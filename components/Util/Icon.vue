<template>
    <i class="material-icons" @click="() => { this.$emit('click') }">
        {{icon}}
    </i>
</template>

<script>
export default {
    props: ['icon']
}
</script>

<style lang="scss" scoped>
i {
    cursor: pointer;
}
</style>