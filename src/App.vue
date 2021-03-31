<template>
  <div id="app">
    <div id="navbar">
      <h1>Courageous Conversations</h1>
    </div>
    <div v-if="user !== '' ">
      <Chat :user="this.user" :user_id="user_id"/>
    </div>
    <div v-if="user === ''">
      <Login v-on:fetchUser="setuser"/>
    </div>
  </div>
</template>

<script>
import Chat from './views/Chat'
import Login from './views/Login'
  export default {
    data() {
      return {
        user: "",
      }
    },
    components: {
      Chat,
      Login
    },
    methods: {
      setuser(data) {
        console.log(data)
        this.user = data.user.username
      }
    }
  }
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Oleo+Script&family=Poppins&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital@0;1&display=swap');
body {
  background-color: hsl(0, 0%, 97%);
  font-family: 'Montserrat', sans-serif;
  #navbar {
    font-family: 'Poppins', sans-serif;
    display: flex;
    flex-direction: row;
    justify-content: center;
    background-color: #DFEDF8;
    box-shadow: 0px 1px 1px 0px rgb(218, 218, 218);

    .link {
      display: inline-block;
    }
  }
#nav {
  display: flex;
  justify-content: space-around;
}
}
</style>
