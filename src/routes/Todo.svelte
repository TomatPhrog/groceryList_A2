<script>
    import linedPage from '$lib/assets/lined-paper-with-paperclip.png';
    let allDone = $state();
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
        checkIfAllChecked()
        return localStorage.setItem("storedList", JSON.stringify(groceryList));
    }

    //add an item to the list
    function addItem(event) {
        event.preventDefault(); //form buttons want to refresh the page and we don't want that
        if (groceryItem.trim().length === 0) {
            checkIfAllChecked()
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
        checkIfAllChecked();
    }

    function clearAll(){
        groceryList = '';
        localStorage.clear();
    }

    function checkIfAllChecked(){
        let completeGrocery = 0;
        groceryList.forEach(listItem => {
            if (listItem.done === true) {
                completeGrocery += 1;
                //console.log("completeGrocery", completeGrocery);
            }
        })
        if (completeGrocery === groceryList.length) {
            //console.log("enable 'clear done' button");
            allDone = true;
        } else {
            allDone = false;
        }
    }
</script>
<!---------------------------------------------------------------------------->
<form onsubmit={addItem}>
    <input type="text" bind:value={groceryItem}>
    <button type="submit">Add</button>
</form>
<div class="grocery-layout">
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
    <div class="sort-options">
        <div class="stickynote">
            <h3>Add items to the list!</h3>
        </div>
        <div class="stickynote">
            <h3>Clear your list once done!</h3>
        </div>
    </div>
</div>
<button onclick={clearAll} disabled={!allDone}>Clear Done</button>

<!---------------------------------------------------------------------------->
<style>
/*https://stackoverflow.com/questions/71760177/styling-the-body-element-in-svelte*/

ul {
    list-style: none;
    padding-left: 1em;
}
li {
    font-size: 1.5em;
    font-weight: 50;
}
li span.done {
    text-decoration: line-through;
    color: grey;
}

input[type="checkbox"] {
    height: 15px;
    width: 15px ;
    accent-color: #178d52;
    &:hover {
        border: #178d52;
        accent-color: #178d5272; 
    }
}
.grocery-layout{
    justify-content: space-between;
    @media (min-width: 825px) {
        display: flex;
    }
    .grocery-list{
        margin-bottom: 3em;
        background-image: src="{linedPage}";
    }
    .sort-options{
        @media (max-width: 825px){
            display: flex;
            justify-content: space-between;
        }
        .stickynote{
            width: 8em;
            height: 8em;
            background-color: #e9d16f;
            margin-top: 1em;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 1em;
        }
    }
}

</style>