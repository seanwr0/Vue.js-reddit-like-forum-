<template>
  <div id="submit-form">
    <h2> SignIn</h2>
    <form @submit="handleSubmit">
      <span>
        <p>Email</p>
        <input type="email" v-model="email" />
      </span>
      <span>
        <p>Password</p>
        <input type="password" v-model="passWord" />
      </span>
      <button>submit</button>
    </form>
  </div>
</template>

<script>
export default {
  name: 'signIn',
  data() {
    return {
      email: "",
      passWord: "",

    }
  },

  methods: {
    /**sends user info to back end to varrify for sign in  */
    handleSubmit() {
      fetch('http://localhost:3000/api/auth/login', {
        method: 'POST',
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          userEmail: this.email,
          password: this.passWord

        })
      })
        .then(response => response.json())
        .then(data => saveToLocalStorage(data))

      function saveToLocalStorage(data) {
        if (data.error) {
          alert(data.error)
        } else {
          let name = JSON.stringify(data.name)
          name = name.replace(/["]+/g, '')
          localStorage.setItem('id', JSON.stringify(data.userId));
          localStorage.setItem('token', JSON.stringify(data.token));
          localStorage.setItem('name', name);
          alert("You are SignedIn")
          window.location.reload()

        }
      }
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
#submit-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 70%;
  min-width: 360px;
  height: 500px;
  background-color: #091f43;

  form {
    display: flex;
    flex-direction: column;
    width: 360px;
    height: 200px;
    gap: 20px;

    span {
      display: flex;
      flex-direction: row;
      height: 30px;
      margin-right: 20px;
    }

    p {
      margin: 0px;
      width: 80px;
    }
  }

  button {
    width: 275px;
    margin-left: 70px;
  }

  input {
    width: 275px;
  }
}
</style>
