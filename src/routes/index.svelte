<script>
    import supabase from "$lib/db";
    import Todo from "$lib/Todo.svelte"
    import {onMount} from "svelte";
    let todos = [];

    onMount(async()=>{
        let { data, error } = await supabase.from('todo').select('*')
        todos = data;
    })
    const updateTodo = async(todo) =>{
        try{
            const { data, error } = await supabase
                .from('todo')
                .update({ client: todo.client })
                .eq('id', todo.id)
        }catch(err) {console.log(err)}
    }
</script>
<h1>Welcome to SvelteKit-todos</h1>
{#each todos as todo }
    <Todo {todo}{updateTodo} />
{:else}
<p>No Clients found </p>
{/each}
<style>

</style>

