<template>
  <div class="container">
    <div class="d-flex justify-content-center align-items-center" style="height: 100vh;">
      <div class="text-left col-md-6 shadow-lg p-5 text-white rounded">
        <div class="text-center">
          <img src="@/assets/logo.png" alt />
          <span class="font-weight-bold pl-2 align-top">E-Commerce</span>
        </div>
        <hr />
        <h2 class="font-weight-bold mb-4">Login</h2>
        <form @submit.prevent="login">
          <div class="form-group">
            <label for="exampleInputEmail1">Email address</label>
            <input
              v-model="email"
              type="email"
              class="form-control"
              id="exampleInputEmail1"
              aria-describedby="emailHelp"
            />
          </div>
          <div class="form-group">
            <label for="exampleInputPassword1">Password</label>
            <input
              v-model="password"
              type="password"
              class="form-control"
              id="exampleInputPassword1"
            />
          </div>
          <div class="text-center">
            <button
              type="submit"
              class="btn btn-primary w-50 font-weight-bold mt-4"
              :disabled="loginProcess"
            >
              <span v-if="loginProcess" class="spinner-border spinner-border-sm mr-2"></span>
              <span>Login</span>
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import api from "../helper/api";
import Swal from "sweetalert2";
export default {
  name: "Home",
  data() {
    return {
      email: "",
      password: "",
      loginProcess: false
    };
  },
  methods: {
    login() {
      this.loginProcess = !this.loginProcess;
      api
        .post("/login", { email: this.email, password: this.password })
        .then(({ data }) => {
          if (data.role === "super_admin" || data.role === "admin") {
            localStorage.setItem("access_token", data.access_token);
            this.$router.push({ name: "Home" });
          } else {
            Swal.fire("Failed", "You cannot access this page", "warning");
          }
          this.loginProcess = !this.loginProcess;
        })
        .catch(err => {
          this.loginProcess = !this.loginProcess;
          Swal.fire("Error", err.response.data.message, "warning");
        });
    }
  }
};
</script>
