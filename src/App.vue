<template>
  <div id="app">
    <b-row>
      <b-col/>
      <b-col sm="10">
        <div id="nav">
          <h1>Rollin'</h1>
        </div>
        <div class="view-container">
          <router-view/>
        </div>
      </b-col>
      <b-col/>
    </b-row>
  </div>
</template>

<style lang="scss">

@import url('https://fonts.googleapis.com/css?family=Pacifico&display=swap');

body {
  background-color: #ffffff;
  background-image: url("./assets/img/pattern.svg");
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
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
