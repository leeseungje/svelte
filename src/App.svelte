<script lang="ts">
    import { writable } from 'svelte/store'
    import Todo from './Todo.svelte'
    let title: string = ''
    let todos = writable([])
    let id = 0

    function createTodo() {
        if(!title.trim()) {
            return title = ''
        }
        $todos.push({
            id,
            title
        })
        todos = todos
        title = ''
        id += 1
        console.log('$todos', todos)
    }
    
</script>

<input bind:value={title} on:keydown={(e) => {
    e.key === "Enter" && createTodo() // 엔터 시
}} type="text" />
<button on:click={createTodo}>
    Create Todo
</button>

<ul>
    {#each $todos as todo }
        <li><Todo {todos} {todo} /></li>   
    {/each}
</ul>