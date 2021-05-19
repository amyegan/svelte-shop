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
  <td class="checkout-item-qty">
    <button
      type="button"
      class="button-small"
      on:click={countButtonHandler}>-</button
    >
    {"  "}
    <span>{count}</span>
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
