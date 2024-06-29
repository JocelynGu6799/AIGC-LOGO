<template>
    <button
      :class="['nor-button', buttonSizeClass]"
      :style="buttonStyles"
    >
      {{ buttonText }}
    </button>
  </template>
  
  <script>
  export default {
    name: 'NorButton',
    props: {
      buttonText: {
        type: String,
        default: '按钮'
      },
      targetColor: {
        type: String,
        default: '#252525'
      },
      size: {
        type: String,
        default: 'medium',
        validator: value => ['small', 'medium', 'large'].includes(value)
      }
    },
    computed: {
      buttonStyles() {
        return {
          '--bg-color': this.color,
          '--target-color': this.targetColor,
          '--hover-bg-color': this.color
        };
      },
      buttonSizeClass() {
        return `size-${this.size}`;
      }
    }
  };
  </script>
  
  <style scoped>
  .nor-button {
    background-color: transparent;
    color: var(--target-color);
    border: 2px solid var(--target-color);
    border-radius: 12px;
    cursor: pointer;
    padding: 8px 40px;
    position: relative;
    transition: background-color 300ms ease, color 300ms ease, border-color 300ms ease;
  }
  
  .nor-button:hover {
    color: white;
    background-color: var(--target-color);
  }
  
  .size-small {
    padding: 4px 20px;
  }
  
  .size-medium {
    padding: 8px 40px;
  }
  
  .size-large {
    padding: 12px 60px;
  }
  </style>
  