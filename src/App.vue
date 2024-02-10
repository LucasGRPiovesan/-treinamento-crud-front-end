<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <nav>
      <router-link to="/funcionarios" active-class="active">Funcionários</router-link>
      <router-link to="/dependentes" active-class="active">Dependentes</router-link>
    </nav>
    <router-view></router-view>
  </div>
</template>

<script>

export default {
  name: 'App',
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
nav {
  width: 70%;
  display: flex;
  margin: 2rem auto;
  justify-content: space-evenly;
}
nav a {
  text-decoration: none;
  color: #34496E;
  font-weight: bold;
}
.active {
  color: #41B883;
  border-bottom: 5px solid #41B883;
}
table {
  border: 1px solid;
}
table thead {
  background-color: gray;
  color: white;
}
td {
  border: 1px solid;
}
th, td {
  padding: .5rem;
}
.componente-form-table {
  width: 100%;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}
</style>
