<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-6">
        <div class="card">
          <div class="card-header">Iniciar sesión</div>
          <div class="card-body">
            <form @submit.prevent="login">
              <div class="mb-3">
                <label for="username" class="form-label"
                  >Correo electrónico</label
                >
                <input
                  class="form-control"
                  id="username"
                  v-model="user"
                  required
                />
              </div>
              <div class="mb-3">
                <label for="password" class="form-label">Contraseña</label>
                <input
                  type="password"
                  class="form-control"
                  id="password"
                  v-model="pacc"
                  required
                />
              </div>
              <button type="submit" class="btn btn-primary">
                Iniciar sesión
              </button>
            </form>
            <div v-if="loginSuccess" class="alert alert-success mt-3">
              ¡Has iniciado sesión con éxito!
            </div>
            <div v-else-if="loginError" class="alert alert-danger mt-3">
              Ha ocurrido un error al iniciar sesión. Inténtalo de nuevo.
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { v4 as uuidv4 } from 'uuid';

export default {
  data() {
    return {
      user: "",
      pacc: "",
      key: "",
      loginSuccess: false,
      loginError: false,
    };
  },
  methods: {
    login() {
      let data = {
        user: this.user,
        pacc: this.pacc,
        key: uuidv4(),//"987342748AC",
      };

      let formData = new URLSearchParams();
      for (let key in data) {
        formData.append(key, data[key]);
      }
      let encodedData = formData.toString();

      axios
        .post("https://jacgsaw.com/auth/login", encodedData)
        .then((response) => {
          const data = response.data;
          if (data.sys.login) {
            this.loginSuccess = true;
            this.loginError = false;
          } else {
            this.loginSuccess = false;
            this.loginError = true;
          }
        })
        .catch((error) => {
          console.error(error);
          this.loginSuccess = false;
          this.loginError = true;
        });
    },
  },
};
</script>

<style scoped>
.container {
  margin-top: 50px;
}

.card-header {
  font-weight: bold;
}

.form-control {
  margin-bottom: 15px;
}
</style>
