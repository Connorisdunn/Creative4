<template>
<div id="app">
  <div class="header">
    <router-link to="/">
      <div class="logo">
        <img src="/chefhat1.png" class="resize">
        <img src="/chefhat1.png" class="resize2">
      </div>
    </router-link>
    <div class="title">
      <h1>Shared Recipes</h1>
    </div>
  </div>
  <div class="content">
    <router-view />
  </div>
  <div class="footer">
    <router-link to="/admin">Add Recipes/Change</router-link>
    <p><a href="https://github.com/Connorisdunn/Creative4">Github</a></p>
	<p>https://github.com/Connorisdunn/Creative4</p>
  </div>
</div>
</template>

<style>
html {
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
  font-size: 16px;
  background: #fff;
  padding: 0px;
  margin: 0px;
}

/* Header */
.header {
  display: float;
  text-align: center;
  margin: 10px 10px 10px 10px;
  padding: 50px 50px 50px 50px;
  background-color: #ffbd5b;
  color: #000417;
}

.title {
  margin-top: 5px;
}

.title h1 {
  font-size: 30px;
}

.content {
  padding: 20px 100px;
  min-height: 500px;
}

/* Footer */
.footer {
  height: 20px;
  text-align: center;
  margin: 5px 5px 5px 5px;
  padding: 25px 25px 25px 25px;
  background: #ffbd5b;
  font-size: 12px;
}

.footer a {
  color: #000417;
}

.resize{
  width: 25%;
  height: 25%;
  position: absolute;
  top: 0px;
  left: 0px;
}
.resize2{
  width: 25%;
  height: 25%;
  position: absolute;
  top: 0px;
  right: 0px;
}

h1 {
  font-size: 25px;
}

h2 {
  font-size: 15px;
}
</style>
