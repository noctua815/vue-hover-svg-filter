<template lang="pug">
main
  Cursor
  router-view(v-slot="{ Component }")
    component(:is="Component")
</template>

<script>
// import MainLayout from '@/components/MainLayout'
import Cursor from '@/components/Cursor'

export default {
  name: 'App',
  components: {
    Cursor
  }
}
</script>

<style>
:root {
  --primary-color: #628395;
  --secondary-color: #262a53;
  --pink-color: #ffa0a0;
  --light-pink-color: #ffe3e3;
  --main-font: tenon, -apple-system, BlinkMacSystemFont, Segoe UI, Helvetica, Arial, sans-serif;
  --special-font: meno-banner, serif;
  --default-font-size: 16px;

  font-size: var(--default-font-size);
}

#app {
  font-family: var(--main-font);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  background-color: var(--light-pink-color);
  cursor: none;
}

*,
*:before,
*:after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
