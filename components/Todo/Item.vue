<template>
    <transition name="topdown" appear>
        <div class="todo-item">
            <div class="todo-content">
                <div class="todo-value">
                    <input :class="{'todo-done': todo.isDone, 'todo-edit': todo.isEditing, 'todo-updating': updating }"
                        type="text" v-model="todo.text" :disabled="!todo.isEditing" @keyup.enter="confirmEdit">
                </div>
            </div>
            <div class="todo-control">
                <transition name="topdown">
                    <div v-if="!todo.isEditing">
                        <util-icon class="edit-button" @click="requestEdit" icon="edit" v-show="!todo.isDone">
                        </util-icon>
                        <util-icon class="remove-button" @click="requestRemove" icon="delete"></util-icon>
                        <util-icon class="done-button" @click="toggleDone" :icon="todo.isDone ? 'remove_done' : 'done'">
                        </util-icon>
                    </div>
                </transition>
                <transition name="topdown">
                    <div v-if="todo.isEditing">
                        <util-icon @click="cancleEdit" icon="undo"></util-icon>
                        <util-icon @click="confirmEdit" icon="check"></util-icon>
                    </div>
                </transition>
            </div>
        </div>
    </transition>
</template>

<script>
export default {
    props: ['id', 'todo'],
    data: () => ({
        backup: "",
        updating: false
    }),
    methods: {
        requestEdit() {
            this.$emit('editing', this.todo)
            this.backup = this.todo.text
        },
        requestRemove() {
            this.$emit('removing', this.todo)
        },
        cancleEdit() {
            this.todo.text = this.backup
            this.todo.isEditing = false
        },
        confirmEdit() {
            this.backup = ""
            this.todo.isEditing = false
            this.updating = true;
            setTimeout(() => {
                this.updating = false;
            }, 500)
        },
        toggleDone() {
            this.todo.isDone = !this.todo.isDone
        }
    }
}
</script>

<style lang="scss" scoped>
.todo-item {
    display: flex;

    .todo-content {
        width: 80%;

        .todo-value {
            height: 100%;

            input {
                width: 100%;
                height: 100%;
                background-color: transparent;
                border: none;
            }
        }
    }

    .todo-control {
        display: flex;
        align-items: center;
        margin-left: 1rem;
        position: relative;

        div {
            display: flex;
            position: absolute;

            i {
                font-size: 2rem;
                margin-inline: .5rem;
                opacity: .25;

                &:hover {
                    opacity: 1;
                }
            }
        }
    }
}

.todo-done {
    text-decoration: line-through;
    color: var(--blue);
}

.todo-edit {
    background-color: white !important;
    outline: 2px solid var(--yellow);
    color: var(--yellow);
}

.todo-updating {
    outline: 2px solid var(--green);
    color: var(--green);
}

.edit-button {
    &:hover {
        color: var(--yellow)
    }
}

.remove-button {
    &:hover {
        color: var(--red)
    }
}

.done-button {
    &:hover {
        color: var(--blue)
    }
}
</style>