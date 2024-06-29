//标签的按钮填充动效
 <template>
  <button class="tag-buttom">
    {{ buttonText }}
  </button>
</template>

<script>
export default {
  props: {
    buttonText: {
      type: String,
      default: '填充按钮' // 默认按钮文本
    }
  },
  data() {
    return {
      hover: false
    };
  }
};
</script>

<style scoped>
button {
  --bg-color: #d9e6ff;
  --font-color: #252525;
  --border-radius: 12px; /* 圆角半径 */
  --border-color: #4768ff;
  background-color: var(--bg-color);
  cursor: pointer;
  clip-path: polygon(0% 0%, 85% 0%, 100% 50%, 85% 100%, 0% 100%); 
  position: relative;
  overflow: hidden; /* 确保子元素在边界内 */
}

button.tag-buttom {
  color: var(--font-color);
  border: 2px solid var(--border-color); 
  border-radius: var(--border-radius); 
  position: relative;
  padding: 8px 40px;
  overflow: hidden; 
}

button.tag-buttom::before {
  content: '';
  position: absolute;
  top: 50%;
  left: 100%;
  width: 200%;
  height: 200%;
  background-color: var(--border-color);
  transition: transform 1000ms ease-out, clip-path 1000ms ease-out;
  transform: translate(-100%, -50%) scale(0);
  transform-origin: right center;
  clip-path: polygon(100% 0, 100% 0, 100% 100%, 100% 100%); 
  z-index: -1;
}

button.tag-buttom:hover::before {
  transform: translate(-100%, -50%) scale(1);
  clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%);
}

button.tag-buttom:hover {
  color: white;
}

button.tag-buttom:not(:hover)::before {
  transform: translate(-100%, -50%) scale(0);
  clip-path: polygon(100% 0, 100% 0, 100% 100%, 100% 100%);
}

button.tag-buttom:not(:hover) {
  color: var(--font-color);
}
</style>
