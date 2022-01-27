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

    $: console.table(todos);
</script>

<input type="text" placeholder="Add a task" bind:value={task} />
<button on:click={addTodo}>Add</button>

<ol>
    {#each todos as item}
        <li class:complete={item.isComplete}>{item.task}</li>
    {/each}
</ol>
