<script>
  import { onMount } from 'svelte';

  let recipes = $state([]);
  let selected = $state(null);
  let name = $state('');
  let ingredients = $state('');
  let instructions = $state('');
  let loaded = false;

  onMount(() => {
    recipes = JSON.parse(localStorage.getItem('recipes') || '[]');
    loaded = true;
  });

  $effect(() => {
    const data = JSON.stringify(recipes);
    if (loaded) localStorage.setItem('recipes', data);
  });

  function addRecipe() {
    if (!name.trim()) return;
    recipes.push({ name: name.trim(), ingredients, instructions });
    name = '';
    ingredients = '';
    instructions = '';
  }

  function deleteRecipe(i) {
    recipes.splice(i, 1);
    selected = null;
  }
</script>

<h1>Cookbook</h1>

<div class="row">
  <div class="img">
    <img src="https://thumbs.dreamstime.com/b/cookbook-25406156.jpg" alt="cooking pot image">
  </div>
</div>

<div class="row">
  <div>
    <h2>Add recipe</h2>
    <input bind:value={name} placeholder="Name" />
    <textarea rows="3" bind:value={ingredients} placeholder="Ingredients"></textarea>
    <textarea rows="4" bind:value={instructions} placeholder="Instructions"></textarea>
    <button onclick={addRecipe}>Add</button>
  </div>

  <div>
    <h2>Recipes</h2>
    <ul>
      {#each recipes as r, i}
        <li>
          <button class="link" onclick={() => (selected = r)}>{r.name}</button>
          <button onclick={() => deleteRecipe(i)}>Delete</button>
        </li>
      {/each}
    </ul>
  </div>
</div>

<h2>Details</h2>
{#if selected}
  <h3>{selected.name}</h3>
  <p>Ingredients:<br />{selected.ingredients}</p>
  <p>Instructions:<br />{selected.instructions}</p>
{:else}
  <p>Click a recipe to view it.</p>
{/if}

<style>
.global{
  font-family: 'Courier New', Courier, monospace;
}
.grid-container{
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-template-rows: repeat(5, 1fr);
        gap: 12px;
        padding: 16px;
        background-color: #fff;
        border-radius: 8px;
        width: 500px;
        aspect-ratio: 1/1;
    }
.img{
  max-width: 320px;
  max-height: 320px;
  width: 100%;
  height: 100%;
}
</style>