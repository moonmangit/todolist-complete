<template>
    <div class="todo-list">
        <todo-input @submit="handleSubmitTodo"></todo-input>
        <todo-item v-for="todo, i in todos" :key="i" :todo="todo" :id="i+1" @editing="handleSetupEditing"
            @removing="handleRemoving" />
    </div>
</template>

<script>
export default {
    data: () => ({
        todos: []
    }),
    methods: {
        handleSubmitTodo(payload) {
            this.todos.push(payload);
        },
        handleSetupEditing(payload) {
            console.log(payload);
            this.todos.forEach(el => {
                el.isEditing = false
            })
            payload.isEditing = true
        },
        handleRemoving(payload) {
            this.todos = this.todos.filter(el => (el.tid !== payload.tid))
        }
    }
}
</script>

<style lang="scss" scoped>
.todo-list {
    display: flex;
    flex-direction: column;

    &>* {
        margin: .5rem;
        flex: 1;
    }
}
</style>