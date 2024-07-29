<script lang="ts">
  let items = [
    { category: "Meat", done: false, text: "Beef", quantity: "500g" },
    { category: "Fruits", done: false, text: "Banana", quantity: "4" },
    { category: "Grain", done: false, text: "Rice", quantity: "10kg" },
    { category: "Fruits", done: false, text: "Apple", quantity: "15" },
  ];

  function addItem(category: string, text: string, quantity: string) {
    items = [...items, { category, done: false, text, quantity }];
  }

  function clear() {
    items = items.filter((t) => !t.done);
  }

  $: remaining = items.filter((t) => !t.done).length;
  $: categoriesWithItems = [...new Set(items.map((item) => item.category))];
  $: sortedItems = items.sort((a, b) => a.text.localeCompare(b.text));

  import { categories } from "./data";
  let selectedCategory = categories[0];
  let newText = "";
  let newQuantity = "";

  function handleSubmit() {
    if (newText && newQuantity) {
      items = [
        ...items,
        {
          category: selectedCategory,
          done: false,
          text: newText,
          quantity: newQuantity,
        },
      ];
      newText = "";
      newQuantity = "";
    } else {
      alert("Please enter product name and quantity!");
    }
  }
</script>

<div class="centered">
  <h1>Shopping List</h1>

  {#each categoriesWithItems as category}
    <h2>{category}</h2>
    <ul class="items">
      {#each items.filter((item) => item.category === category) as todo}
        <li class:done={todo.done}>
          <input type="checkbox" bind:checked={todo.done} />

          <input
            class="name"
            type="text"
            placeholder="What needs to buy?"
            bind:value={todo.text}
          />

          <input
            class="quantity"
            type="text"
            placeholder="Quantity"
            bind:value={todo.quantity}
          />
        </li>
      {/each}
    </ul>
  {/each}

  <p>{remaining} remaining</p>

  <div class="form-container">
    <form on:submit|preventDefault={handleSubmit}>
      <div class="form-inputs">
        <select bind:value={selectedCategory}>
          {#each categories as category}
            <option value={category}>{category}</option>
          {/each}
        </select>
        <input type="text" bind:value={newText} placeholder="Item name" />
        <input
          class="input-quantity"
          type="text"
          bind:value={newQuantity}
          placeholder="Quantity"
        />
      </div>
      <button type="submit">Add</button>
    </form>
    <button on:click={clear}> Clear completed </button>
  </div>
</div>

<style>
  .centered {
    max-width: 20em;
    margin: 0 auto;
  }

  .done {
    opacity: 0.6;
  }

  ul {
    margin-left: -20px;
  }

  li {
    display: flex;
    margin-bottom: 10px;
  }

  .name {
    padding: 0.5em;
    border: none;
    border-radius: 5px;
    margin: 2px;
    width: 150px;
  }

  .quantity {
    padding: 0.5em;
    border: none;
    border-radius: 5px;
    margin: 2px;
    width: 60px;
    text-align: center;
  }

  .form-container {
    display: flex;
    flex-direction: column;
  }

  .form-inputs {
    display: flex;
    justify-content: space-between;
  }

  button[type="submit"] {
    margin: 10px;
  }

  .input-quantity {
    width: 80px;
  }
</style>
