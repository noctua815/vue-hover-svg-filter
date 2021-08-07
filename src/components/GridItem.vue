<template lang="pug">
.item(ref="el")
  figure.thumb(@mouseover="mouseEnter" @mouseleave="mouseLeave")
    svg(class="distort" width="350" height="450" viewBox="0 0 350 450" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink")
      filter(:id="`distortionFilter${index}`")
        feTurbulence(type="fractalNoise" baseFrequency="0" numOctaves="1" result="warp")
        feDisplacementMap(xChannelSelector="R" yChannelSelector="G" scale="250" in="SourceGraphic")
      image(:filter="`url(#distortionFilter${index})`" class="distort__img" x="50" y="50" :xlink:href="data.img" height="350" width="250")
    figcaption.thumb__caption
      p.thumb__caption-title Prow scuttle parrel provost Sail ho shrouds spirits boom mizzenmast yardarm.
      a.thumb__caption-link Explore
  .item__meta
    .item__meta-counter {{ computedIndex }}
    h3.item__meta-title.link {{ data.title }}
    .item__meta-detail {{ data.country }}
    .item__meta-detail {{ data.year }}
</template>

<script>
import {gsap} from 'gsap'
let tl = null

export default {
  name: 'item',
  props: {
    data: {
      type: Object,
      default: () => ({})
    },
    index: {
      type: Number,
      default: 0
    },
    filterType: {
      type: String,
      default: 'turbulence'
    }
  },
  watch: {},
  data() {
    return {
      DOM: {},
      primitiveValues: {}
    }
  },
  computed: {
    computedIndex() {
      return this.index >= 10 ? this.index : '0' + this.index
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.getDOM()
    })
  },
  methods: {
    getDOM() {
      this.DOM.el = this.$refs.el
      this.DOM.thumb = this.DOM.el.querySelector('.thumb')
      this.DOM.thumbSVG = this.DOM.thumb.querySelector('.distort')
      this.DOM.SVGFilter = this.DOM.thumbSVG.querySelector('filter')
      this.DOM.SVGImage = this.DOM.thumbSVG.querySelector('image.distort__img')
      gsap.set(this.DOM.SVGImage, {transformOrigin: ' 50% 50%'})

      this.DOM.feTurbulence = this.DOM.SVGFilter.querySelector('feTurbulence')
      this.DOM.feDisplacementMap = this.DOM.SVGFilter.querySelector('feDisplacementMap')
      this.primitiveValues = this.filterType === 'turbulence' ? {baseFrequency: 0} : {scale: 0}

      this.DOM.caption = this.DOM.thumb.querySelector('.thumb__caption')
      this.DOM.captionTitle = this.DOM.caption.querySelector('.thumb__caption-title')
      this.DOM.captionLink = this.DOM.caption.querySelector('.thumb__caption-link')

      this.DOM.meta = this.DOM.el.querySelector('.item__meta')
      this.DOM.metaCounter = this.DOM.meta.querySelector('.item__meta-counter')
      this.DOM.metaTitle = this.DOM.meta.querySelector('.item__meta-title')
      this.DOM.metaDetail = [...this.DOM.meta.querySelectorAll('.item__meta-detail')]

      this.createHoverTimeline()
    },
    createHoverTimeline() {
      tl = gsap.timeline({
        paused: true,
        defaults: {
          duration: 0.7,
          ease: 'power2'
        },
        onUpdate: () => this.updateFilterValues(),
        onReverseComplete: () => {
          if (this.filterType === 'turbulence') {
            this.primitiveValues.baseFrequency = 0
            this.updateFilterValues()
          }
        }
      })

      if (this.filterType === 'turbulence') {
        tl.to(this.primitiveValues, {
          startAt: {baseFrequency: 0.09},
          baseFrequency: 0
        }, 0)
      } else {
        tl.to(this.primitiveValues, {
          // (displacementMap) scale start value
          startAt: {scale: 0},
          scale: 150
        }, 0)
      }

      tl
          .to(this.DOM.caption, {
            y: '0%'
          }, 0)
          .to([this.DOM.captionTitle, this.DOM.captionLink], {
            y: 0,
            startAt: {y: 100, opacity: 0},
            opacity: 1,
            stagger: 0.1
          }, 0)
          .to([this.DOM.metaCounter, this.DOM.metaTitle, this.DOM.metaDetail], {
            duration: 0.1,
            x: i => i % 2 === 0 ? '-5%' : '5%',
            opacity: 0,
            stagger: 0.05
          }, 0)
          .to([this.DOM.metaCounter, this.DOM.metaTitle, this.DOM.metaDetail], {
            duration: 0.5,
            ease: 'power3',
            startAt: {x: i => i % 2 === 0 ? '15%' : '-15%'},
            x: '0%',
            opacity: 1,
            stagger: 0.08
          }, 0.1)

      if (navigator.userAgent.indexOf('Firefox') === -1) {
        tl.to(this.DOM.SVGImage, {
          scale: 1.2
          //y: '-50%'
        }, 0)
      }
    },
    mouseEnter() {
      tl.restart()
    },
    mouseLeave() {
      tl.reverse()
    },
    updateFilterValues() {
      if (this.filterType === 'turbulence') {
        this.DOM.feTurbulence.setAttribute('baseFrequency', this.primitiveValues.baseFrequency)
        console.log('updateFilterValues', this.filterType, this.primitiveValues.baseFrequency)

      } else {
        this.DOM.feDisplacementMap.setAttribute('scale', this.primitiveValues.scale)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.item {
  &__preview {
    height: 200px;
    //background-color: var(--pink-color);
  }

  &__preview {
    //margin-bottom: 1rem;
  }

  &__meta {
    font-size: .85rem;
    margin-top: 1rem;
    padding-bottom: 1rem;
    color: var(--secondary-color);
    border-bottom: 1px solid var(--secondary-color);

    &-counter {
      margin-bottom: 1rem;
    }

    &-title {
      margin-bottom: 1rem;
      font-family: var(--special-font);
      font-size: 1.75rem;

    }

    &-detail {
      margin-bottom: 1rem;
    }
  }
}

.thumb {
  position: relative;
  width: 200px;
  height: 220px;
  margin: 0;
  overflow: hidden;

  &__caption {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    padding: 1rem;
    background: var(--secondary-color);
    transform: translate3d(0, 100%, 0);

    &-title {
      margin-bottom: 0.75rem;
      color: var(--light-pink-color);
    }

    &-link {
      display: block;
      cursor: pointer;
      color: var(--pink-color);
    }
  }
}

.distort {
  pointer-events: none;
  margin: -115px 0 0 -75px;
}
</style>
