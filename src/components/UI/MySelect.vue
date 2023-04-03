<template>
    <div class="my-select">
      <div class="my-select__placeholder" v-if="!selectedValue">{{ placeholderText }}</div>
      <select class="my-select__select" :value="selectedValue" @change="changeOption">
        <option v-for="option in options" 
        :key="option.id"
        :value="option.id">{{ hasName ? option.name : option.id }}</option>
      </select>
    </div>
  </template>
  
  <script>
  export default {
    name: 'my-select',
    props: {
      modelValue: {
        type: String,
        default: null,
      },
      options: {
        type: Array,
        default: () => [{ name: 'Выберите из предыдущего списка' }],
      },
      placeholderText: {
        type: String,
        default: 'Choose from list...',
      },
    },
    data() {
      return {
        selectedValue: this.modelValue,
      };
    },
    methods: {
      changeOption(event) {
        this.selectedValue = event.target.value;
        this.$emit('update:modelValue', this.selectedValue);
      },
    },
    computed: {
      hasName() {
        return 'name' in this.options[0];
      },
    },
  };
  </script>
  
  <style lang="scss" scoped>
  .my-select {
    position: relative;
    width: 100%;
    height: $base-ui-height;
    &__placeholder {
        position: absolute;
        left: 10px;
        top: 50%;
        transform: translateY(-50%);
      color: $select-text;
      //color:  $input-placeholder;
        pointer-events: none;
      font-family: $base-font-family;

      }
      
      &__select {
        font-family: $base-font-family;
        font-size: $base-font-size;
        color: $option-color-font;
        width: 100%;
        height: 100%;
        padding: 10px;
      border-radius: 15px;
      background: $input-background;    
        font-family: sans-serif;
      }
  }
  
  option {
    font-size: $option-font-size;
    color: $option-color-font;
  }

  </style>