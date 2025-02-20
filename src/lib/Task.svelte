<script>
    export let task;
    export let onDragStart;
    export let onDragOver;
    export let onDrop;

    import { tasks } from "../store.js";

    function toggleComplete() {
      tasks.update(t => 
        t.map(item => item.id === task.id ? { ...item, completed: !item.completed } : item)
      );
    }

    function deleteTask() {
      tasks.update(t => t.filter(item => item.id !== task.id));
    }
</script>

<div class="task-card"
    draggable="true"
    on:dragstart={() => onDragStart(task.id)}
    on:dragover={onDragOver}
    on:drop={() => onDrop(task.id)}
    on:click={toggleComplete}
    class:completed={task.completed}
>
    <p>{task.text}</p>
    <button class="delete-button" on:click={deleteTask}>X</button>
</div>

<style>
    .task-card {
        display: flex;
        flex-direction: column; 
        justify-content: center; 
        align-items: center;
        position: relative;
        border-radius: 12px;
        background: #222;
        color: white;
        box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
        cursor: grab;
        font-size: 0.875rem;
        width: 10rem;
        height: 10rem;
        transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
        aspect-ratio: 1; 
    }


    .task-card:active {
        cursor: grabbing;
    }

    .task-card.completed {
        text-decoration: line-through;
        background: #555;
        color: #bbb;
    }

    .task-card:hover {
        box-shadow: 2px 4px 20px rgba(0, 0, 0, 0.2);
    }

    .task-card:focus {
        outline: 2px solid #888;
    }

    .delete-button {
        position: absolute;
        top: 0.5rem;
        right: 0.5rem;
        background: #e74c3c;
        color: white;
        border: none;
        border-radius: 8px;
        padding: 0.25rem 0.5rem;
        cursor: pointer;
        font-size: 0.875rem;
        transition: background-color 0.3s ease;
    }

    .delete-button:hover {
        background: #c0392b;
    }
</style>