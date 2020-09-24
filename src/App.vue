<template>
  <div id="app">
    <!-- <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div> -->
    <router-view/>
  </div>
</template>

<style>
*{margin:0;padding:0}

li{list-style:none}

img{vertical-align:top;border:none}

body {
  text-align:center;
  background:#ccc;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  /* padding-left:20px;
  padding-right:20px;
  padding-top:20px;
  padding-bottom: 20px; */
}
#app{
  text-align: left;
  margin:auto;
  width:600px;
  margin-top:20px;
  margin-bottom:20px;
  box-shadow:2px 2px 5px #4c5e70;
  background-color:#f7f8fa;
  min-height:1000px;
  border-radius: 5px;
}

@media screen and (max-width:1000px){
  body{
    background-color:#ffffff;
  }
  #app{
    margin-top:0;
    width:100%;
    border:none;
    box-shadow:none;
    border-radius: none;
  }
}
.page{
padding: 0 16px;

}
.padding{
  padding:0 16px;
}
.title{
    margin: 0;
    padding: 32px 16px 16px;
    color: rgba(69, 90, 100, 0.6);
    font-weight: normal;
    font-size: 14px;
    line-height: 16px;
}
#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
