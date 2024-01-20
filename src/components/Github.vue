<template>
  <div class="">
    <div class="form-group row">
      <label class="sr-only col col-form-label" for="githubUser">Usuario de github:</label>
      <input placeholder="Introduzca el nombre de usuario" id="githubUser" class="col form-control" type="text"
        :disabled="disabled" v-on:keydown.enter="obtenerUsuario()" v-model="user">
    </div>
  </div>
  <div class="row alert alert-danger" role="alert" v-if="isError">
    El usuario no existe
  </div>


  <div class="row-justify-content-center">
    <div v-if="validUser" class="card col-sm-4">
      <img :src="userData.avatar_url" class="img-fluid card-img-top" alt="avatar">
      <div class="card-body">
        <h5 class="card-title"> {{ userData.name }} </h5>
        <button v-on:click="obtenerRepos()" class="btn btn-primary">Repositorios</button>
        <a class="card-link" :href="userData.html_url">Url de github</a>
      </div>
    </div>

    <div class="col-sm-8" v-if="showRepos">
      <div class="list-group">
        <GitHubRepo v-for="repo in repos" :key="repo.id" :repo="repo"></GitHubRepo>
      </div>
    </div>

  </div>
</template>

<script>

import GitHubRepo from './GitHubRepo.vue'


export default {
  name: 'Github',
  components: {
    GitHubRepo
  },
  data: function () {
    return {
      user: "",
      disabled: false,
      isError: false,
      validUser: false,
      repos: [],
      showRepos: false
    }
  },
  methods: {
    obtenerUsuario() {

      this.disabled = true;
      this.isError = false;
      //this.showRepos= false;

      var url = `https://api.github.com/users/${this.user}`;

      fetch(url)
        .then(response => {
          if (response.ok) {
            return response.json();
          } else {
            throw new Error("Usuario no encontrado");
          }

        }).then(
          datos => {
            this.userData = datos;
            this.validUser = true;
          }
        )
        .catch(error => {
          console.log(error);
          this.validUser = false;
          this.isError = true;
        }).finally(() => {
          this.disabled = false;
        })

    },

    obtenerRepos: function () {


      //this.showRepos= false;

      var url = this.userData.repos_url;

      fetch(url)
        .then(response => {
          if (response.ok) {
            return response.json();
          } else {
            throw new Error("Usuario no encontrado");
          }

        }).then(
          datos => {
            this.repos = datos;
            this.showRepos = true;
          }
        )
        .catch(error => {
          console.log(error);
          this.validUser = false;
          this.showRepos = false;

          this.isError = true;
        })
    }
  },
}
</script>

<style></style>
