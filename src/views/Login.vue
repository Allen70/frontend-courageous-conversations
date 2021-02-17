<template>
  <div id="app">
        <div v-if="is_login" >
          <button @click="changeState">Login</button>
          <form @submit="signUp">
              <label>Username</label>
              <input type="text" name="username" v-model="username">
              <label>Password</label>
              <input type="password" name="password" v-model="password">
              <input type="submit" value="Signup">
          </form>
        </div>
        <div v-else >
          <button @click="changeState">Signup</button>
          <form @submit="login">
              <label>Username</label>
              <input type="text" name="username" v-model="username">
              <label>Password</label>
              <input type="password" name="password" v-model="password">
              <input type="submit" value="Login">
          </form>
        </div>
  </div>
</template>

<script>

  export default {
    name: 'Login',
    data() {
      return {
        is_login: false,
        username: '',
        password: '',
      }
    },
    methods: {
        signUp(event) {
            event.preventDefault()
            fetch('http://localhost:4000/users',{
              method: "POST",
              headers: {'Content-Type':'application/json'},
              body: JSON.stringify({
                username: this.username,
                password: this.password,
              })
            })
                .then(response => response.json())
                .then(data => {
                    this.$emit('fetchUser',data)
                })
        },
        login(event) {
          event.preventDefault()
          fetch('http://localhost:4000/login',{
            method: "POST",
            headers: {'Content-Type':'application/json'},
            body: JSON.stringify({
              username: this.username,
              password: this.password,
            })
          })
          .then(response => response.json())
          .then(data => {
            this.$emit('fetchUser',data)
          })
        },
        changeState(){
          this.is_login = !this.is_login
        }
    }
  }
</script>

<style lang="scss" scoped>
button {
  position: absolute;
  top: 25%;
  left: 50%;
  transform: translate(-262%);
}
form {
    position: absolute;
    top: 30%;
    left: 50%;
    transform: translate(-50%);
    display: flex;
    flex-direction: column;
    width: 300px;
    height: fit-content;

}

label, input{
    display: inline-block;
}


</style>
