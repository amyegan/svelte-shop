<script>
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
        <br />
        <button type="button" on:click={checkout}>
          Checkout
        </button>
      {/if}
    </div>
</div>
