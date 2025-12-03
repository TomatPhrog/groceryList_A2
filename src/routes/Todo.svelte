<script>
    let clearDone = document.querySelector('#clear-done');
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
        checkIfAllChecked();
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
        let eachArrGrocery = 0;
        console.log("checking");
        groceryList.forEach(listItem => {
            console.log("for eaching");
            if (!groceryList.done) {
                eachArrGrocery += 1;
                //console.log("groc arr", eachArrGrocery);
                //console.log("array", groceryList.length);
            }
        })
        if (eachArrGrocery === groceryList.length) {
            clearDone.classList.remove("disabled");
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
            <h3>checked</h3>
        </div>
        <div class="stickynote">
            <h3>trash</h3>
        </div>
    </div>
</div>
<button id="clear-done" onclick={clearAll} disabled>Clear Done</button>

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
    @media (min-width: 470px) {
        display: flex;
    }
    .grocery-list{
        background-image: src="../lib/assets/blank-lined-paper.png";
    }
    .sort-options{
        @media (max-width: 649px){
            display: flex;
        }
        .stickynote{
            width: 8em;
            height: 8em;
            background-color: #e9d16f;
        }
    }
}

</style>