<script>
export default {
  name: "Login",
  data() {
    return {
      email: "",
      password: "",
      message: "",
      loading: false,
      auth: false
    }
  },
  methods: {
    async handleLogin() {
      this.loading = true;
      const response = await fetch('http://127.0.0.1:8000/api/login', {
        method: 'POST',
        headers: {
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({ "email": this.email, "password": this.password })
      });
      this.loading = false;
      const status = await response.json();
      this.message = status;
      console.log(status);
      if (status.data.token) {
        const d = new Date();
        d.setTime(d.getTime() + (60 * 60));
        this.setCookie('token', status.data.token, 1);
      }
    },
    setCookie(cname, cvalue, hours) {
      const d = new Date();
      d.setTime(d.getTime() + (hours * 60 * 60 * 1000));
      let expires = "expires=" + d.toUTCString();
      document.cookie = cname + "=" + cvalue + ";" + expires + ";path=/";
    },
    getCookie(cname) {
      let name = cname + "=";
      let decodedCookie = decodeURIComponent(document.cookie);
      let ca = decodedCookie.split(';');
      for (let i = 0; i < ca.length; i++) {
        let c = ca[i];
        while (c.charAt(0) == ' ') {
          c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
          return c.substring(name.length, c.length);
        }
      }
      return "";
    },
    checkToken(){
      if( this.getCookie('token') !== "" ){
      this.$router.push({name:'create'});
    }
    }
  },
  mounted() {
    this.checkToken()
  },
  updated(){
    this.checkToken()
  }
}
</script>
<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-12 text-center mt-2">
        <h1>Login</h1>
      </div>
    </div>
    <div class="row">
      <div class="col-md-12 p-3">
        <div class="card card-container p-3">
          <img id="profile-img" src="@/assets/avatar.png" width="100" class="profile-img-card mx-auto" />
          <form name="form" @submit.prevent="handleLogin">
            <div class="form-group mb-3">
              <label for="email">Email</label>
              <input v-model="email" required type="text" class="form-control" name="email" />
              <!-- <div class="alert alert-danger" role="alert">Email is required!</div> -->
            </div>
            <div class="form-group mb-3">
              <label for="password">Password</label>
              <input v-model="password" required type="password" class="form-control" name="password" />
              <!-- <div class="alert alert-danger" role="alert">Password is required!</div> -->
            </div>
            <div class="form-group mb-3">
              <button class="btn btn-primary btn-block" :disabled="loading">
                <span v-show="loading" class="spinner-border spinner-border-sm"></span>
                <span>Login</span>
              </button>
            </div>
            <div class="form-group">
              <div v-if="message" class="alert alert-danger" role="alert">{{ message }}</div>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
<style>
</style>