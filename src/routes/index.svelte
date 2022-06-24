<script>
    import supabase from "$lib/db";
    import Todo from "$lib/Todo.svelte"
    import {onMount} from "svelte";
    let todos = [];
    let newClient = "";

    onMount(async()=>{
        await getAllTodo();
    })

    const getAllTodo = async() =>{
        try{let { data, error } = await supabase.from('todo').select('*')
        todos = data;
        }catch (err){
            console.log(err)
        }
    }
    const addNewClient = async() =>{
        try{
            const { data, error } = await supabase
                .from('todo')
                .insert([{ client: newClient }])
            await getAllTodo();
            newClient = "";
        }catch(err) {console.log(err)}
    }
    const updateTodo = async(todo) =>{
        try{
            const { data, error } = await supabase
                .from('todo')
                .update({ client: todo.client ,isComplete: todo.isComplete})
                .eq('id', todo.id)
            await getAllTodo();
        }catch(err) {console.log(err)}
    }
    const deleteTodo = async(todo) =>{
        try{
            const { data, error } = await supabase
                .from('todo')
                .delete()
                .eq('id', todo.id)
            await getAllTodo();
        }catch(err) {console.log(err)}
    }
    const handleKeyPress = (event) =>{
        if(event.key === "Enter" && newClient !== ""){
            addNewClient();
        }
    }
</script>
<h1>Welcome to SvelteKit-todos</h1>
<div class="add-todo">
    <input type="text" bind:value={newClient}>
    <button on:click={() => addNewClient()}>Add Client</button>
</div>
{#each todos as todo }
    <Todo {todo}{updateTodo} {deleteTodo}/>
{:else}
<p>No Clients found </p>
{/each}
<svelte:window on:keypress={handleKeyPress} />
<style>
    .add-todo{
        display: flex;
        margin-bottom: 0.5em;
    }
</style>

