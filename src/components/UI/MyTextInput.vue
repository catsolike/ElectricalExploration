<template>
    <input  class="input-text" 
            type="text"
            :value="modelValue"
            @input="updateInput"        
            >
</template>

<script>

export default {
    name: 'my-text-input',
    props:{
        modelValue: [String, Number]
    },
    methods:{
        updateInput(event) { 
            this.$emit('update:modelValue', event.target.value)
        }
    }
}
</script>

<style lang="scss" scoped>
.input-text {
    height: 5.5vh;
    width: 100%;
    font-size: $base-font-size;
    padding: 10px 15px;
    color: $input-text;
    background: $input-background;

    border-radius: 15px;
}

.input-text::placeholder{
    color: $input-placeholder;
    font-size: $option-font-size;
}

</style>