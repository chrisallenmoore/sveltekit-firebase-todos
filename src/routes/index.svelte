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
        todos = [todo, ...todos];
        task = "";
    };

    const markTodoAsComplete = (index) => {
        todos[index].isComplete = !todos[index].isComplete;
        console.log("wtf");
    };

    $: console.table(todos);
</script>

<input type="text" placeholder="Add a task" bind:value={task} />
<button on:click={addTodo}>Add</button>
<div>stuff here</div>

<ol>
    {#each todos as item, index}
        <li class:complete={item.isComplete}>
            <span>{item.task}</span>
            <span
                ><button on:click={() => markTodoAsComplete(index)}>✔</button
                ></span
            >
        </li>
    {/each}
</ol>

<style>
    .complete {
        text-decoration: line-through;
    }
</style>
