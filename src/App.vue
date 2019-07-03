<template>
  <div id="app">
    <div id="nav">
      <h1>Rollin'</h1>
    </div>
    <div class="view-container">
      <router-view/>
    </div>
  </div>
</template>

<style lang="scss">

@import url('https://fonts.googleapis.com/css?family=Pacifico&display=swap');

body {
  background-color: #ffffff;
  background-image: url("/img/pattern.svg");
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  margin-left: 5em;
  margin-right: 5em;
}

#nav {

  font-family: 'Pacifico', cursive;

  h1 {
    margin-top: 0.5rem;
    margin-bottom: 0rem;
  }

  a {
    font-weight: bold;
    color: #2c3e50;
    &.router-link-exact-active {
      color: #69399a;
    }
  }
}

.view-container {
  background: white;
  padding: 2em;
  border-style: solid;
  border-width: 1px;

}

</style>
