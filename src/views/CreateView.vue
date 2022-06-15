<script>
export default {
  name: "create-pic",
  data() {
    return {
      title: "",
      description: "",
      url: "",
      author: "",
      like: "",
      loading: false
    }
  },
  methods: {
    async addPicture(){
     // alert("Picture added ? Nope. but you have a token " + this.getToken()) https://httpbin.org/post 
      this.loading = true;
      const response = await fetch('http://127.0.0.1:8000/api/authpicture', {
        method: 'POST',
        headers: {
          'Accept': 'application/json',
          'Content-Type':'application/json',
          'Authorization':'Bearer ' + this.getToken()
        },
        body: JSON.stringify({
          title: this.title,
          url: this.url,
          description: this.description,
          author: this.author,
          like: this.like
        })
      });
      this.loading = false;
      const status = await response.json();
      console.log(status);
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
    getToken() {
      let token = this.getCookie("token").split("|");
      token = token[1];
      return token;
    }
  }
}
</script>
<template>
  <div class="container-fluid">
    <div class="row">
      <h1>You can add a new Picture !</h1>
    </div>
    <form class="row g-3 mb-3" @submit.prevent="addPicture">
      <div class="col-12">
        <label for="title" class="form-label">Title</label>
        <input v-model="title" required type="text" class="form-control" id="title" name="title">
      </div>
      <div class="col-12">
        <label for="url" class="form-label">Url</label>
        <input v-model="url" required type="text" class="form-control" id="url" name="url">
      </div>
      <div class="col-12">
        <label for="description" class="form-label">Description</label>
        <textarea v-model="description" required class="form-control" id="description" name="description">
    </textarea>
      </div>
      <div class="col-12">
        <label for="author" class="form-label">Author</label>
        <input v-model="author" required type="text" class="form-control" id="author" name="author">
      </div>
      <div class="col-12">
        <label for="like" class="form-label">Like</label>
        <input v-model="like" required class="form-control" type="text" id="like" name="like">
      </div>
      <div class="col-12">
        <button type="submit" class="btn btn-primary" :disabled="loading">
          <span v-show="loading" class="spinner-border spinner-border-sm"></span>
          Add Picture
        </button>
      </div>
    </form>
  </div>
</template>
<style>
</style>
