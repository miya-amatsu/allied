<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/collect">collect</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style lang="scss">
@import url(scss/normalize.css);
@import url(scss/dist/main.css);

</style>
