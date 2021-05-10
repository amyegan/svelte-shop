<script>
  import Card from "./Card.svelte";
  import items from "../Stores/items.js";
</script>

<div class="container">
  <div class="row">
  
  {#each items as item}
    <div class="card-wrapper">
      <Card {item} /> 
    </div> 
  {/each} 
 </div>
</div> 
