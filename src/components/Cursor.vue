<template lang="pug">
svg(class="cursor" width="40" height="40" viewBox="0 0 40 40" ref="cursor")
  circle.cursor__inner(cx="20" cy="20" r="10")
</template>

<script>
import gsap from 'gsap'

export default {
  name: '',
  props: {},
  watch: {},
  data() {
    return {
      mouse: {
        x: 0,
        y: 0
      },
      renderedStyles: {
        tx: {previous: 0, current: 0, amt: 0.15},
        ty: {previous: 0, current: 0, amt: 0.15},
        scale: {previous: 1, current: 1, amt: 0.15},
        opacity: {previous: 1, current: 1, amt: 0.1}
      },
      el: null,
      targets: ['a', '.link']
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.initCursor()
      setTimeout(() => {
        this.initTargets()
      }, 100)
    })
  },
  methods: {
    initCursor() {
      this.el = this.$refs.cursor
      this.bounds = this.el.getBoundingClientRect()
      window.addEventListener('mousemove', e => {
        this.mouse = this.getMousePos(e)
      })
      window.addEventListener('mousemove', this.onMouseMoveEv)
    },
    onMouseMoveEv() {
      this.renderedStyles.tx.previous = this.renderedStyles.tx.current = this.mouse.x - this.bounds.width / 2
      this.renderedStyles.ty.previous = this.renderedStyles.ty.current = this.mouse.y - this.bounds.height / 2
      gsap.to(this.el, {duration: 0.9, ease: 'Power3.easeOut', opacity: 1})
      requestAnimationFrame(() => this.render())
      window.removeEventListener('mousemove', this.onMouseMoveEv)
    },
    render() {
      this.renderedStyles.tx.current = this.mouse.x - this.bounds.width / 2
      this.renderedStyles.ty.current = this.mouse.y - this.bounds.height / 2

      for (const key in this.renderedStyles) {
        this.renderedStyles[key].previous = this.lerp(this.renderedStyles[key].previous, this.renderedStyles[key].current, this.renderedStyles[key].amt)
      }
      this.el.style.transform = `translateX(${ this.renderedStyles.tx.previous }px) translateY(${ this.renderedStyles.ty.previous }px) scale(${ this.renderedStyles.scale.previous })`
      this.el.style.opacity = this.renderedStyles.opacity.previous

      requestAnimationFrame(() => this.render())
    },
    getMousePos(e) {
      return {
        x: e.clientX,
        y: e.clientY
      }
    },
    lerp(a, b, n) {
      return (1 - n) * a + n * b
    },
    enter() {
      this.renderedStyles.scale.current = 1.8
      this.renderedStyles.opacity.current = 0.8
    },
    leave() {
      this.renderedStyles.scale.current = 1
      this.renderedStyles.opacity.current = 1
    },
    initTargets() {
      [...document.querySelectorAll(this.targets)].forEach(item => {
        item.addEventListener('mouseenter', () => this.enter())
        item.addEventListener('mouseleave', () => this.leave())
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.cursor {
  display: none;
}

@media (any-pointer: fine) {
  .cursor {
    position: fixed;
    top: 0;
    left: 0;
    display: block;
    pointer-events: none;
    z-index: 10000;
    opacity: 0;

    &__inner {
      //fill: var(--secondary-color);
      fill: var(--pink-color);
    }
  }
}
</style>
