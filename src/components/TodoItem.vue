<template>
    <div class="todo-item" v-bind:class="{'is-complete':todo.completed}">
        <p>
            <input type="checkbox" v-on:change="markComplete" />
            {{todo.title}}
            <button @click="$emit('del-todo', todo.id)">x</button>
        </p>
    </div>
</template>

<script>
    // import axios from 'axios';
    export default {
        name: "TodoItem",
        props: ["todo"],
        methods: {
            markComplete() {
                this.$emit("mark-complete", this.todo.id);
                // this.todo.completed = !this.todo.completed;
            }
        }
    }
</script>

<style scoped>
    .todo-item {
        background: #fff4ff;
        padding: 10px;
        border-bottom: 1px #ccc dotted;
    }
    .is-complete {
        text-decoration: line-through;
    }
</style>