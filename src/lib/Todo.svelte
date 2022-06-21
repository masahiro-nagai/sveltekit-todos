<script>
    export let todo,updateTodo;
    
</script>
<div class="todos">
    <input type="checkbox" checked={todo.isComplete} />
    <input type="text"
        value={todo.client} 
        on:input={(e)=>{
        todo.client = e.currentTarget.value;
        updateTodo(todo);
    }}
    />
    <button>Delete</button>
</div>
<style>
    .todos{
        display: flex;
        margin-bottom: 0.25em;
    }
</style>