<template>
    <div    class="dialog" 
            v-if="show" 
            @click="hideDialog"
    >
        <div class="dialog__content"
             @click.stop
        >
            <slot></slot>
        </div>
    </div>
</template>

<script>
export default {
    name: 'my-dialog',
    props: {
        show: {
            type: Boolean,
            default: false
        }
    },
    methods: {
        hideDialog() {
            this.$emit('update:show', false)
        }
    }
}
</script>

<style lang="scss" scoped>
.dialog { 
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    background: $dialog-background;
    position: fixed;
    display: flex;

    &__content {
        display: flex;
        align-items: center;
        justify-content: center;

        padding: 20px;
        margin: auto;
        background: $dialog-content-background;
        border-radius: 12px;
        min-height: 30vh;
        min-width: 30vw;
    }
}

</style>