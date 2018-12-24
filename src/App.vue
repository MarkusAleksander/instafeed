<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style lang="scss">
html,
body {
  width: 100%;
  height: 100%;
}

body,
body *,
body *:after,
body *:before {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
