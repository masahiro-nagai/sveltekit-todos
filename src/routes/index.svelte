<script>
    import supabase from "$lib/db";
    import {user} from "$lib/stores";
    import Todo from "$lib/Todo.svelte"
    import {onMount} from "svelte";
    import {goto} from "$app/navigation";

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
                .insert([{ client: newClient, user_id: $user.id }])
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
    const logOut = async() =>{
        let { error } = await supabase.auth.signOut();
        $user = false;
        goto("/login");
    }
    $:console.log($user);
</script>
<h4>Welcome {$user?.email ? $user.email :""}!</h4>
<div class="add-todo">
    <input type="text" bind:value={newClient}>
    <button on:click={() => addNewClient()}>Add Client</button>
</div>
{#each todos as todo }
    <Todo {todo}{updateTodo} {deleteTodo}/>
{:else}
<p>No Clients found </p>
{/each}
{#if $user.email}
    <p on:click={logOut} class="switch">Logout</p>
{/if}
<svelte:window on:keypress={handleKeyPress} />
<style>
    .add-todo{
        display: flex;
        margin-bottom: 0.5em;
    }
    :global(.switch){
        color: lightskyblue;
        cursor: pointer;
    }
    :global(.switch:hover){
        text-decoration: underline;
    }
</style>

