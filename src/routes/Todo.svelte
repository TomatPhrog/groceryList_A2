<script>
    import {onMount} from 'svelte';

    let groceryItem = $state('');
    let groceryList = $state([]);

    onMount(() => {
        let storedList = localStorage.getItem("storedList");
        if (storedList) {
            groceryList = (JSON.parse(storedList));
        }
    })

    function updateList() {
        return localStorage.setItem("storedList", JSON.stringify(groceryList));
    }

    //add an item to the list
    function addItem(event) {
        event.preventDefault(); //form buttons want to refresh the page and we don't want that
        if (groceryItem.trim().length === 0) {
            return; //if the item is empty it cannot be added to the list
        }

        groceryList = [...groceryList, {
            text: groceryItem,
            done: false
        }];
        updateList();
        groceryItem = '';
    }

    function removeItem(index) {
        groceryList.splice(index,1);
        updateList();
    }

    function clearAll(){
        groceryList = '';
        localStorage.clear();
    }

</script>

<form onsubmit={addItem}>
    <input type="text" bind:value={groceryItem}>
    <button type="submit">Add</button>
</form>

<div class="grocery-list">
    <ul>
        {#each groceryList as item, index}
        <li>
            <input type="checkbox" bind:checked={item.done} onchange={updateList}>
            <span class:done={item.done}>{item.text}</span>
            <button type="button" onclick={() => removeItem(index)}>&times;</button>
        </li>
        {/each}
    </ul>
</div>

<button onclick={clearAll}>Clear List</button>


<style>
/*https://stackoverflow.com/questions/71760177/styling-the-body-element-in-svelte*/
:global(body) {
    font-family: "Cherry Bomb One", sans-serif;
}
ul {
    list-style: none;
}
li span.done {
    text-decoration: line-through;
    color: grey;
}
</style>