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

<svg viewBox="0 0 240 180" role="img" aria-label="A blue pot on a yellow background">
  <rect width="240" height="180" fill="#f2d64b" />
  <rect x="48" y="70" width="144" height="12" rx="6" fill="#2b2e6b" />
  <path d="M56 82h128v42a30 30 0 0 1-30 30H86a30 30 0 0 1-30-30z" fill="#3b3f8f" />
  <rect x="24" y="92" width="32" height="10" rx="5" fill="#2b2e6b" />
  <rect x="184" y="92" width="32" height="10" rx="5" fill="#2b2e6b" />
</svg>

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
  :global(body) { font-family: sans-serif; max-width: 900px; margin: 0 auto; padding: 1rem; }
  svg { width: 100%; max-width: 320px; height: auto; }
  .row { display: flex; flex-wrap: wrap; gap: 1rem; }
  .row > div { flex: 1 1 300px; }
  input, textarea { display: block; width: 100%; margin-bottom: 0.5rem; box-sizing: border-box; }
  p { white-space: pre-wrap; }
  .link { background: none; border: none; color: blue; text-decoration: underline; cursor: pointer; }
</style>