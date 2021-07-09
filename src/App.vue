<template>
  <div id="app">
    <div id="nav" class="navbbar">
      <div class="markerdiv"><i class="fas fa-bookmark marker"></i></div>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link> |
    </div>

    <router-view />
  </div>
</template>

<script>
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: #000b13d2;
  background: linear-gradient(circle, #0d3653d2, #1c2536);

  height: 100vh;
}

#nav {
  /* padding: 30px; */
  /*   float: right; */
}

#nav a {
  font-weight: bold;
  color: #0b1014;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
.navbbar {
  background-color: white;
  width: 300px;
  height: 50px;
  margin: auto;
}
.marker {
  color: red;
  font-size: 20px;
}
.markerdiv {
  text-align: right;
  margin-right: 30px;
}
</style>

