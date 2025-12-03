<script>
    import Todo from './Todo.svelte';
    import basket from '$lib/assets/shoppingBasket.webp';
    
</script>
<!-------------------------------------------------------------------------------------->
<header>
    <img class="logo" src="{basket}" alt="shopping basket">
</header>
<h1>Grocery List App!</h1>

<Todo />

<!-------------------------------------------------------------------------------------->
<style>
:global(body) {
    font-family: "Cherry Bomb One", sans-serif;
    margin: 0%;
    padding: 0em 3em;
    @media (min-width: 825px){
        padding: 0em 12em;
    }
     @media (min-width: 1400px){
        padding: 0em 20em;
    }
     @media (min-width: 1600px){
        padding: 0em 25em;
    }
}
h1 {
    font-size: 3em;
    display: flex;
    justify-content: center;
    margin-top: 0%;
}
p {
    font-size: 1.5em;
}
header {
    padding: 1em;
}
.logo{
    display: flex;
    justify-content: flex-start;
    width: 15%;
}
</style>