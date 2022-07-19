<script>
// @ts-nocheck

import { gun } from "./initGun"
let input = ""
const create = () => gun.get("todos").get(input).put({ title: input, done: false })
let store = {}
gun.get("todos").map().on(function(data, key) {
    if (data) {
        store[key] = data
    } else {
        delete store[key]
        store = store
    }
})
$: todos = Object.entries(store).sort((a, b) => a[1].done)
</script>

<input placeholder="Add todo" bind:value={input} />
<button on:click={() => create() && (input = "")}>Add</button>

{#each todos as [key, todo]}
<div id={key}>
    {todo.title} {todo.done ? "😺" : "😾"}
</div>
{/each}