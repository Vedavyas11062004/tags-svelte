<script>
  // @ts-nocheck

  import { onMount } from "svelte";

  let filter = ["test"];
  let input;
  let selectedElements = [];

  function showFilter() {
    const filterElement = document.getElementById("filter");
    if (filterElement) {
      filterElement.innerHTML = filter.join(", ");
    }
  }
  function toggleSelect(tag) {
    if (selectedElements.includes(tag)) {
      selectedElements = selectedElements.filter((e) => e !== tag);
    } else {
      selectedElements = [...selectedElements, tag];
    }
  }

  function addTag(val) {
    if (!filter.includes(val)) {
      filter = [...filter, val];
    } else {
      document.querySelectorAll(".tag").forEach((tag) => {
        if (tag.textContent == val) {
          tag.classList.add("animated", "bounce");
        }
      });
    }
    showFilter();
  }

  function removeTag(text) {
    filter = filter.filter((tag) => tag !== text);
    showFilter();
  }

  onMount(() => {
    input.addEventListener("keydown", (event) => {
      if (event.which === 13) {
        event.preventDefault();
        const val = input.value
          .replace(/ /g, "")
          .replace(/[^a-z0-9\s]/gi, "")
          .replace(/[_\s]/g, "-")
          .toLowerCase();
        if (val == "") return false;

        addTag(val);
        input.value = "";
      }
    });

    input.addEventListener("keydown", (event) => {
      if (event.which == 8 && input.value == "") {
        const touch = input.previousElementSibling;
        const text = touch.textContent;
        removeTag(text);
        touch.parentElement.removeChild(touch);
      }
    });

    input.addEventListener("click", () => input.focus());
  });

  function handleClick(innerText) {
    // const innerText = this.innerText;
    const val = innerText
      .replace(/ /g, "")
      .replace(/[^a-z0-9\s]/gi, "")
      .replace(/[_\s]/g, "-")
      .toLowerCase();
    if (val == "") return false;

    addTag(val);
    input.value = "";
  }

  let searchTerm = "";
  let items = [
    "Element 1",
    "Element 2",
    "Element 3",
    // Add more items as needed
  ];

  let filteredItems = items;

  function filterItems() {
    filteredItems = items.filter((item) =>
      item.toLowerCase().includes(searchTerm.toLowerCase())
    );
  }
</script>

<div class="container">
<div class="searchBar">
  {#each filter as tag (tag)}
    <span
      class={selectedElements.includes(tag) ? "tag selected" : "tag"}
      on:click={() => toggleSelect(tag)}
    >
      {tag}
      <span class="remove" on:click={() => removeTag(tag)} />
    </span>
  {/each}
  <input bind:this={input} type="text" name="search" placeholder="" size="1" />
</div>
<div class="searchList">
  <div>
    <input
      type="text"
      bind:value={searchTerm}
      placeholder="Search..."
      on:input={filterItems}
      class="searchInput"
    />
  </div>

  <ul class="elements">
    {#each filteredItems as item (item)}
      <li class="element" on:click={() => handleClick(item)}>{item}</li>
    {/each}
  </ul>
</div>
<div class="--debug">
  <h3 style="color: white; font-family: 'Roboto'">
    var Filter : <span id="filter" />
  </h3>
</div>
</div>
<style>
  *{
    font-family: sans-serif;
  }
  .container{
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    gap: 1rem;
    width: 85vw;
    margin-inline: auto;
    margin-top: 5rem;
  }
  .searchBar {
    background: white;
    border: 1px solid black;
    box-sizing: content-box;
    padding: 5px 10px;
    border-radius: 4px;
    font-family: Roboto;
    line-height: 50px;
    width: 80vw;
    /* margin: 10vh auto; */
    cursor: pointer;
    transition: 0.1s all;
  }

  .searchBar .tag {
    background: #333;
    margin: 5px;
    color: white;
    padding: 10px 15px 10px 20px;
    border-radius: 4px;
    cursor: pointer;
  }

  .searchBar .tag:hover {
    background: #222;
  }

  .searchBar .tag .remove:before {
    content: "×";
    padding-left: 15px;
  }

  .searchBar input[name="search"] {
    padding: 10px;
    border: 1px solid transparent;
    font-size: 16px;
    background: white;
  }
  .elements {
    list-style: none;
    padding: 0%;
    display: flex;
    flex-direction: column;
    gap: .5rem;
  }
  .elements:hover{
    cursor: pointer;
  }
  .searchList{
    border: 1px solid black;
    width: fit-content;
    padding: 1rem;
  }
  .searchInput{
    height: 30px;
  }
</style>
