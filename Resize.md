### Resize
监测resize事件的方式很多，如：
- `window.resize`：窗口 `resize`，只能监测到元素的宽度变化
- [`scroll`](https://zhuanlan.zhihu.com/p/24887312)：ElementUI 早期采用的方式
- [`ResizeObserver`](https://developers.google.com/web/updates/2016/06/performance-observer)：ElementUI 现在采用方式

```js
import { addResizeListener, removeResizeListener } from '@/utils/resize-event';

export default {
  computed: {
    containerRef() {
      return this.$refs.container;
    },
  },
  mounted() {
    this.$nextTick(() => this.setupResize());
  },
  methods: {
    setupResize() {
      const handler = () => console.log('resize');
      addEventListener(this.containerRef, handler); 
      this.$once('hook:beforeDestroy', () => {
        removeResizeListener(this.containerRef, handler);
      });
    },
  },
  render(h) {
    return h('div', {
      ref: 'container'
    }, 'hello');
  },
};
```