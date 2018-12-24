<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Montserrat");

html {
  font-size: 62.5%;

  &,
  body {
    width: 100%;
    height: 100%;
  }
}

body {
  & *,
  & *:after,
  & *:before {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: "Montserrat";
    line-height: 1.2;
    font-size: 2rem;
    text-align: center;
    color: #000;
  }
}
</style>
