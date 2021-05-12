<script>
  import { loadScript } from "@paypal/paypal-js";
  import CheckoutItem from "./CheckoutItem.svelte";
  import { cart } from "../Stores/stores.js";
  let checkedOut = false;
  let cartItems = [];

  const unsubscribe = cart.subscribe((items) => {
    cartItems = Object.values(items);
  });

  const checkout = () => {
    checkedOut = true;
    cart.update((n) => {
      return {};
    });
  };

  let totalPrice = 1;
  $: if (cartItems.length) {
    totalPrice = 0;
    cartItems.forEach((item) => {
      const itemSubtotal = Number(item.price) * item.count;
      console.log("item subtotal", itemSubtotal);
      totalPrice += itemSubtotal;
    });
  }

  loadScript({ "client-id": "test" })
    .then((paypal) => {
      paypal
        .Buttons({
          createOrder: function (data, actions) {
            return actions.order.create({
              purchase_units: [
                {
                  amount: {
                    value: totalPrice,
                  },
                },
              ],
            });
          },
          onApprove: function (data, actions) {
            // This function captures the funds from the transaction.
            return actions.order.capture().then(function (details) {
              // This function shows a transaction success message to your buyer.
              checkout();
            });
          },
        })
        .render("#paypal-button");
    })
    .catch((err) => {
      console.error("Failed to load the PayPal JS SDK script", err);
    });
</script>

<div class="container">
  <h1>My Cart</h1>

  <div class="col-sm">
    {#if cartItems.length === 0}
      {#if checkedOut}
        <p class="empty-message">Thank you for shopping with us</p>
      {:else}
        <p class="empty-message">Your cart is empty</p>
      {/if}
    {:else}
      <table>
        {#each cartItems as item (item.name)}
          <CheckoutItem {item} />
        {/each}
      </table>
      <div class="checkout-review">
        <p>Total: $ {totalPrice}</p>
        <div id="paypal-button" />
      </div>
    {/if}
  </div>
</div>
