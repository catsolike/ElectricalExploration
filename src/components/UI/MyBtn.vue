<template>
    <button class="btn">
        <slot></slot>
    </button>
</template>

<script>
export default {
    name: 'my-btn'
}
</script>

<style lang="scss" scoped>
.btn {
    padding: 10px 15px;
    font-size: $base-font-size;
    background: $btn-background;
    color: $btn-text;
    border-radius: 15px;
    height: fit-content;
    min-height: 5.5vh;
    width: 6vh;
    min-width: fit-content;
}
</style>