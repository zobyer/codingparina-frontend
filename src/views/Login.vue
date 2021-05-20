<template>
  <navbar />
  <div v-if="!spin_visible" class="add_container">
    <h1>Login</h1>
    <form @submit.prevent="submitcredentials">
      <div class="container">
        <div class="box">
          <label><b>Email</b></label>
          <input type="text" placeholder="Enter Email .." v-model="email" />
          <label class="error" v-if="email == ''">Email Can't be Empty</label>
        </div>

        <div class="box">
          <label><b>Password</b></label>
          <input
            type="password"
            placeholder="Enter Password"
            v-model="password"
          />
          <label class="error" v-if="password == ''">Password Can't be Empty</label>
        </div>
      </div>
      <button class="login">Add Device</button>
    </form>
  </div>
  <div v-if="spin_visible" id="spinner">
    <Circle8></Circle8>
  </div>
</template>

<script >
import axios from "axios";
import Swal from "sweetalert2";
import Navbar from '../components/Navbar.vue';
import {Circle8} from 'vue-loading-spinner'

export default {
  components: {Navbar, Circle8},
  data() {
    return {
      email: "",
      password: "",
      spin_visible: false,
    };
  },
  methods: {
    submitcredentials() {
        if (this.email == "" || this.password == "") return;
        this.spin_visible = true;
      axios
        .post("https://codingparina.herokuapp.com/api/login", {
          email: this.email,
          password: this.password,
        })
        .then((response) => {
          localStorage.setItem("token", response.data.token);
          localStorage.setItem("isloggedin", true);
          localStorage.setItem("user_id",response.data.user.id);
          localStorage.setItem("user_name",response.data.user.name);
          this.spin_visible = false;
          this.$router.push({ name: "Home" });

          //console.log(localStorage.getItem("user_id"),response.data.user.id);
        })
        .catch((err) => {
          localStorage.setItem("isloggedin", false);
          this.spin_visible = false;
          this.failedsweetalert();
          //console.log(err);
        });
    },
    failedsweetalert() {
      //console.log(localStorage.getItem("islogged"));
      if (localStorage.getItem("isloggedin") == "false") {
        Swal.fire({
          position: "top-end",
          width: 400,
          icon: "error",
          title: "Login Failed! Incorrect Credentials",
          showConfirmButton: false,
          timer: 2000,
        });
      }
    },
  },
  created() {
    localStorage.setItem("isloggedin", false);
  },
};
</script>

<style scoped>
.add_container {
  width: 40%;
}
@media screen and (max-width: 800px) {
  .add_container {
    width: 70%;
  }
}
</style>