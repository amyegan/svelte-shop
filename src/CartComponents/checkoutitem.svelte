<script>
  import { cart } from "../Stores/stores.js";
  export let item;
  let { name, price, img, count } = item;
  const countButtonHandler = (e) => {
    if (e.target.classList.contains("add")) {
      count++;
    } else if (count >= 1) {
      count--;
    }
    cart.update((n) => ({ ...n, [name]: { ...n[name], count } }));
  };
  const removeItem = () => {
    cart.update((n) => {
      delete n[name];
      return n;
    });
  };
  $: itemSubtotal = price * count;
</script>

<tr>
  <td><img
    class="img-fluid img-thumbnail"
    width="300"
    src={`img/${img}`}
    alt={name}
  /></td>
  <td class="checkout-item-name">{name}</td>
  <td>
    <button
      type="button"
      class="button-small"
      on:click={countButtonHandler}>-</button
    >
    {"  "}
    <span class="checkout-item-qty">{count}</span>
    {"  "}
    <button
      type="button"
      class="button-small add"
      on:click={countButtonHandler}>+</button
    >
  <td class="checkout-item-price">Price: $ {itemSubtotal}</td>
  <td><button type="button" class="btn btn-danger" on:click={removeItem}>
    Remove
  </button></td>
</tr>

<style>
  table {
    width: 100%;
  }

  tr {
    vertical-align: middle;
  }

  td:first-child, th:first-child {
    padding-left: 0;
  }

  td, th {
      border-bottom: 0.1rem solid #e1e1e1;
      padding: 1.2rem 1.5rem;
  }

  .checkout-item-name {
    font-size: 1.6rem;
    letter-spacing: .03rem;
  }

  .checkout-item-price {
    font-size: 1.1rem;
  }

  .checkout-item-qty {
    font-weight: 700;
  }

  button.button-small {
    line-height: 0;
    padding: .7rem;
    text-align: center;
    border-radius: 2rem;
} 
</style>