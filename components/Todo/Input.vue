<template>
    <div class="todo-input">
        <input :class="{inputValid: isInputValid}" v-model="userIn" type="text" placeholder="to do staff..."
            @keyup.enter="submitTodo">
        <util-icon :class="{hidden: !isInputValid}" @click="submitTodo" icon="add_circle" />
    </div>
</template>

<script>
export default {
    data: () => ({
        userIn: "",
    }),
    computed: {
        isInputValid() {
            return (this.userIn !== "")
        }
    },
    methods: {
        submitTodo() {
            let sending = this.userIn.trim()
            if (sending !== "") {
                this.$emit('submit', {
                    tid: parseInt(Math.random() * 100000000),
                    text: sending,
                    isEditing: false,
                    isDone: false
                })
            }
            this.userIn = ""
        }
    }
}
</script>

<style lang="scss" scoped>
.todo-input {
    display: flex;
    overflow: hidden;
    align-items: center;
}

input {
    flex: 1;
    z-index: 10;
    background-color: var(--secondary);
    color: var(--primary);
    padding-inline: 1rem;

    &.inputValid {
        flex: .95;
    }
}

i {
    font-size: 2.4rem;
    position: absolute;
    right: 0;

    &:hover {
        color: var(--green);
    }

    &.hidden {
        transform: translateX(-100%);
    }
}
</style>