<template>
    <div>
            <li>
                <span v-bind:class='{done: todo.completed}'>
                    <input type="checkbox" v-on:change='todo.completed = !todo.completed'>
                    <strong>{{index+1}}</strong>
                    {{todo.title | uppercase}}
                </span>
                <button class='rm'
                     v-on:click="$emit('remove-todo', todo.id)"
                >&times;</button>
            </li>
    </div>
</template>

<script>
export default {
    props: {
        todo: {
            type: Object,
            required: true
        },
        index: Number
    },
    filters: {
        uppercase(value){
            return value.toUpperCase()
        }
    }
}
</script>

<style scoped>
    li {
        display: flex;
        border: 1px solid #ccc;
        justify-content: space-between;
        padding: 5px 15px;
        margin-bottom: 5px;
    }

    .rm {
        border-radius: 50%;
        background-color: red;
        font-weight: bold;
    }

    input {
        margin-right: 10px;
    }

    .done {
        text-decoration: line-through;
    }
    
</style>