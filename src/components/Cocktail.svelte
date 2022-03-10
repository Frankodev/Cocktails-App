<script lang="ts">
  import {fade} from 'svelte/transition';
  export let drinkData: any;
</script>

<div transition:fade="{{delay: 250, duration: 300}}" class="card mx-2 p-0" style="width: 18rem;">
  <img loading="lazy" src={drinkData.strDrinkThumb} class="card-img-top" alt={drinkData.strDrink}>
  <div class="card-body">
    <h5 class="card-title">{drinkData.strDrink}</h5>
    <p class="card-text">{drinkData.strInstructions}</p>
  </div>
</div>