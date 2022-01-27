<script>
    import { initializeApp, getApps, getApp } from "firebase/app";
    import {
        getFirestore,
        collection,
        onSnapshot,
        doc,
        updateDoc,
        deleteDoc,
        addDoc,
        enableIndexedDbPersistence,
    } from "firebase/firestore";
    import { firebaseConfig } from "$lib/firebaseConfig.js";
    import { browser } from "$app/env";

    const firebaseApp =
        browser &&
        (getApps().length === 0 ? initializeApp(firebaseConfig) : getApp());
    const db = browser && getFirestore();

    browser &&
        enableIndexedDbPersistence(db).catch((err) => {
            if (err.code == "failed-precondition") {
                // Multiple tabs open, persistence can only be enabled
                // in one tab at a a time.
                // ...
            } else if (err.code == "unimplemented") {
                // The current browser does not support all of the
                // features required to enable persistence
                // ...
            }
        });

    const colRef = browser && collection(db, "todos");

    let todos = [];

    const unsubscribe =
        browser &&
        onSnapshot(colRef, (querySnapshot) => {
            let fbTodos = [];
            querySnapshot.forEach((doc) => {
                let todo = { ...doc.data(), id: doc.id };
                fbTodos = [todo, ...fbTodos];
            });
            //console.table(fbTodos);
            todos = fbTodos;
        });
    /**
     * let todos = [
     *   { task: "Get milk", isComplete: false, createdAt: "2022-01-27"},]
     */
    let task = "";
    let error = "";

    const addTodo = async () => {
        if (task !== "") {
            const docRef = await addDoc(collection(db, "todos"), {
                task: task,
                isComplete: false,
                createdAt: new Date(),
            });
            task = "";
            error = "";
        } else {
            error = "Task is empty";
        }
    };

    const markTodoAsComplete = async (item) => {
        await updateDoc(doc(db, "todos", item.id), {
            isComplete: !item.isComplete,
        });
    };

    const deleteTodo = async (id) => {
        await deleteDoc(doc(db, "todos", id));
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
                ><button on:click={() => markTodoAsComplete(item)}>✔</button
                ></span
            >
            <span><button on:click={() => deleteTodo(item.id)}>✘</button></span>
        </li>
    {:else}
        <li>No todos found</li>
    {/each}
    <p class="error">{error}</p>
</ol>
<svelte:window on:keydown={keyIsPressed} />

<style>
    .complete {
        text-decoration: line-through;
    }
    .error {
        color: red;
    }
</style>
