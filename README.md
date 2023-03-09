# Svelte

## Store

- 상태 관리 라이브러리를 Svelte에선 `Store.js`를 사용 한다.
- `store.js`

```javascript
import { writable } from 'svelte/store'

export let storeName = writable('leeseungje') // 초기값
```

- `App.svelte`

```javascript
<script>
	import { storeName } from './store.js' // storeName 호출
	import Child from './Child.svelte'

	let name = 'World!'
	$storeName = name // leeseungje = 'World!' 바뀜
	// $ : Auto-subscription(자동 구독)
</script>

<h1>Hello {name}</h1> // Hello World!
<Child />
```

- `Child.svelte`

```javascript
<script>
    import { storeName } from './store.js'
</script>

<div>
    Child {$storeName} // Child World!
</div>
```

## typescript

- svelte 파일에서 <script> 코드 실행 시 `lang="ts"` 실행하면 typescript도 구현 가능 하다.

```javascript
<script lang="ts">let name: string = 'name'</script>
```

## 조건문과 반복문

- 조건문은 `{#if}` 로 시작해 `{/if}`로 끝나여 elese 일 경우 `{:else}`를 넣어 준다.
- 반복문은 react나 javascript에서 map이나 foreach또는 다양한 방식이 있지만.
- svelte에서는 `{#each  as }`를 사용

### 조건 문

```javascript
let name = "world"
let toggle = false
// javascript 문법
if(toggle) {

} else {

}

// svelte 문법
{#if toggle}
    <h1>Hello {name}</h1>
{:else}
    <div>no name!!</div>
{/if}
```

### 반복문

- `App.svelte`

```javascript
<script>
    /**
     * Fruits.svelte 를 import
    */
	import Fruits from './Fruits.svelte'
	let fruits = ['Apple', 'Banana', 'Cherry', 'Orange', 'Mango']
</script>

<Fruits {fruits} /> // fruits 배열 props
<Fruits {fruits}  slice="-2" /> // fruits 배열, slice string props
<Fruits {fruits} reverse /> // true값 props
<Fruits {fruits}  slice="0, 3" />
```

- `Fruits.svelte`

```javascript
<script>
    // props
    export let fruits  // props Fruits
    export let reverse // props reverse
    export let slice // props slice
    // stats
    let computedFruits = []

    if(reverse) {
        computedFruits = [...fruits].reverse() // 전개 연산자
        // fruits.reverse() 할 경우 원본 데이터 복제되므로 deepcopy를 해야 하므로 전개 연산자를 이용한다.
    } else if(slice) {
        computedFruits = fruits.slice(...slice.split(','))
    } else {
        computedFruits = fruits
    }
</script>

<h2>Fruits {reverse ? 'reverse' : ''}</h2>
<ul>
	{#each computedFruits as item }
		<li>{item}</li>
	{/each}
</ul>
```

## Todo 리스트

- `App.svelte`

```javascript
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
```

- `Todo.svelte`

```javascript
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
```
