<script>
    let todos = [];
    /**
     * let todos = [
     *   { task: "Get milk", isComplete: false, createdAt: "2022-01-27"},]
     */
    let task = "";

    const addTodo = () => {
        let todo = {
            task: task,
            isComplete: false,
            createdAt: new Date(),
        };
        if (task !== "") {
            todos = [todo, ...todos];
            task = "";
        }
    };

    const markTodoAsComplete = (index) => {
        todos[index].isComplete = !todos[index].isComplete;
    };

    const deleteTodo = (index) => {
        let deleteItem = todos[index];
        todos = todos.filter((item) => item != deleteItem);
    };

    const keyIsPressed = (event) => {
        if (event.key === "Enter") {
            addTodo();
            console.log("Enter pressed");
        }
        //console.log(event);
    };

    $: console.table(todos);
</script>

<input type="text" placeholder="Add a task" bind:value={task} />
<button on:click={addTodo}>Add</button>

<ol>
    {#each todos as item, index}
        <li class:complete={item.isComplete}>
            <span>{item.task}</span>
            <span
                ><button on:click={() => markTodoAsComplete(index)}>✔</button
                ></span
            >
            <span><button on:click={() => deleteTodo(index)}>✘</button></span>
        </li>
    {:else}
        <li>No todos found</li>
    {/each}
</ol>
<svelte:window on:keydown={keyIsPressed} />

<style>
    .complete {
        text-decoration: line-through;
    }
</style>
