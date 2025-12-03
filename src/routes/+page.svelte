<script>
    import Todo from './Todo.svelte';
</script>
<!-------------------------------------------------------------------------------------->
<header>
    <p class="logo">Company Logo</p>
</header>
<h1>Grocery List App!</h1>

<Todo />

<!-------------------------------------------------------------------------------------->
<style>
:global(body) {
    font-family: "Cherry Bomb One", sans-serif;
    margin: 0%;
    padding: 0em 5em;
    @media (min-width: 470px){
        padding: 0em 20em;
    }
}
h1 {
    font-size: 3em;
}
p {
    font-size: 1.5em;
}
header {
    padding: 1em;
}
.logo{
    display: flex;
    justify-content: center;
}
</style>