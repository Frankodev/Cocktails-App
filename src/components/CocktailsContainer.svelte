<script lang="ts">
  import { dataset_dev, element } from 'svelte/internal';
  import axios from 'axios';
  import Cocktail from './Cocktail.svelte';
  import ErrorAlert from './ErrorAlert.svelte';

  let drinkName: string = '';
  let drinks: Array<any> = [];

  const getDrinks = async (drinkSearch: string = 'margarita') => {
    
    const {data} = await axios.get(`https://www.thecocktaildb.com/api/json/v1/1/search.php?s=${drinkSearch}`);
    
    drinks = data.drinks;

    if(drinkSearch.trim() === '') {
      throw new Error('Ingresa una bebida en el buscador');
    } else if(drinks === null) {
      throw new Error('No encontramos resultados');
    }
    
    console.log('Array', drinks);
  };

  let promise = getDrinks();
  
  const handleSubmit = () => {
    promise = getDrinks(drinkName);
    drinkName = '';
  };
</script>

<main class="container mt-3 text-center d-flex flex-column justify-content-center align-items-center"> 
  <h4 class="pb-3 text-white">Search Cocktails</h4>
  <form on:submit|preventDefault={handleSubmit} class="mb-5 d-inline-flex gap-3 align-items-center justify-content-center">
    <input bind:value={drinkName} class=" form-control" type="text">
    <button type="submit" class="btn btn btn-outline-info">Search</button>
  </form>

  <!-- el await espera a que se resuelva la promesa -->
  
  {#await promise}
  <div class="d-flex justify-content-center align-items-center spinner-grow text-light" role="status">
    <span class="visually-hidden">Loading...</span>
  </div>
    
  <!-- el then requiere de un renombre -->
  {:then drinksResponse} 

  <section class=" p-3 row gap-3 justify-content-center">
    {#each drinks as drink}
    
      <Cocktail drinkData={drink}/>
    
    {/each}
  </section>

  {:catch error}
  <ErrorAlert {error}/>

  {/await}

</main>

<!-- https://svelte.dev/examples/onmount -->

