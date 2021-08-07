<template lang="pug">
.layout
  .title Hildegard Rost
  .content
    grid-item(v-for="(item, i) of list"
      :data="item"
      :filterType="item.filter"
      :index="i + 1")
</template>

<script>
import GridItem from '@/components/GridItem'
export default {
  components: {
    GridItem
  },
  name: 'MainPage',
  props: {},
  watch: {},
  data() {
    return {
      list: [
        {
          title: 'Some title',
          year: 2020,
          country: 'Paris, France',
          img: '/images/img-1.jpeg',
          filter: 'turbulence'
        },
        // {
        //   title: 'Some title',
        //   year: 2020,
        //   country: 'Paris, France',
        //   img: '/images/img-1.jpeg',
        //   filter: 'displacementMap'
        // },
        // {
        //   title: 'Some title',
        //   year: 2020,
        //   country: 'Paris, France',
        //   img: '/images/img-1.jpeg',
        //   filter: 'displacementMap'
        //
        // },
        // {
        //   title: 'Some title',
        //   year: 2020,
        //   country: 'Paris, France',
        //   img: '/images/img-1.jpeg',
        //   filter: 'displacementMap'
        // },
        // {
        //   title: 'Some title',
        //   year: 2020,
        //   country: 'Paris, France',
        //   img: '/images/img-1.jpeg'
        // },
        // {
        //   title: 'Some title',
        //   year: 2020,
        //   country: 'Paris, France',
        //   img: '/images/img-1.jpeg'
        // }
      ]
    }
  },
  computed: {},
  methods: {}
}
</script>

<style lang="scss" scoped>
.layout {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

.title {
  width: 100%;
  padding: 2vh 0;
  font-family: var(--special-font);
  font-size: 16vw;
  text-align: center;
  color: var(--secondary-color);
}

.content {
  display: grid;
  grid-template-columns: repeat(5, minmax(200px, 1fr));
  grid-template-rows: 500px;
  gap: 1rem;
  padding: 1rem 2rem;
}

.item {
  &:nth-child(odd) {
    padding-top: 10vh;
  }
}
</style>
