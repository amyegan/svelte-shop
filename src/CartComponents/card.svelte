<script>
  import { get } from "svelte/store";
  import { cart } from "../Stores/stores.js";
  export let item;
  let { img, name, price } = item;
  img = `img/${img}`;
  const cartItems = get(cart);
  let inCart = cartItems[name] ? cartItems[name].count : 0;
  function addToCart() {
    inCart++;
    cart.update(n => {
      return { ...n, [name]: { ...item, count: inCart } };
    });
  }
</script>



<div class="card">
  <img  class="card-img-top" width="200" src={img} alt={name} />
  <div class="card-body">

    <div>
      <h2 class="card-title">{name}</h2>
      <strong class="price"> $ {price}</strong>
    </div>

    <button type="button center" class="btn btn-primary" on:click={addToCart}>
      Add to cart
    </button>
    <span class="item-cart-indicator">
      {#if inCart > 0}
        <em>({inCart} in cart)</em>
      {/if}
    </span>
  </div>
</div>