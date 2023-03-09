<script>
    export let todos
    export let todo

    let isEdit = false
    let title = ''

    function handleEdit() {
        isEdit = true
        title = todo.title
    }
    function handleDelete() {
        $todos = $todos.filter(t => t.id !== todo.id)
        console.log(todos)
    }
    function handleUpdate() {
        todo.title = title
        handleCancel()
    }
    function handleCancel() {
        isEdit = false
    }
</script>

{#if isEdit}
    <div>
        <input bind:value={title} on:keydown={(e) => e.key === "Enter" && handleUpdate()} type=text />
        <button on:click={handleUpdate}>
            OK
        </button>
        <button on:click={handleCancel}>
            Cancel
        </button>
    </div>
{:else}
    <div>
        {todo.title}
        <button on:click={handleEdit}>
            Edit
        </button>
        <button on:click={handleDelete}>
            Delete
        </button>
    </div>
{/if}

<style></style>