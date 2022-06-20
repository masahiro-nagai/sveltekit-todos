<script>
    import supabase from "$lib/db";
    import {onMount} from "svelte";
    let todos = [];

    onMount(async()=>{
        let { data, error } = await supabase.from('todo').select('*')
        todos = data;
    })
</script>
<h1>Welcome to SvelteKit-todos</h1>
{#each todos as todo }
    <div class="todos">
        <input type="checkbox" checked={todo.isComplete}>
        <input type="text" value={todo.client}>
        <button>Delete</button>
    </div>
{:else}
<p>No Clients found </p>
{/each}
<style>
    .todos{
        display: flex;
        margin-bottom: 0.25em;
    }
</style>

