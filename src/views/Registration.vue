<template>
  <navbar />
  <div v-if="!spin_visible" class="add_container">
    <h1>Login</h1>
    <form @submit.prevent="emailavailable">
      <div class="container">
        <div class="box">
          <label><b>User Name</b></label>
          <input
            type="text"
            placeholder="Enter User Name .."
            v-model="username"
            required
          />
        </div>

        <div class="box">
          <label><b>Email</b></label>
          <input
            type="email"
            placeholder="Enter Email .."
            v-model="email"
            required
          />
          
        </div>

        <div class="box">
          <label><b>Password</b></label>
          <input
            type="password"
            placeholder="Enter Password"
            v-model="password"
            required
            @keyup="show_meter = true"
          />
          <div class="pass_meter">
            <label v-if="show_meter" class="error">Password Strength Meter </label>
            <password-meter :password="password" />
          </div>

        </div>

        <div class="box">
          <label><b>Confirm Password</b></label>
          <input
            type="password"
            placeholder="Confirm Password"
            v-model="confirm_password"
            required
          />
          <label class="error" v-if="password != confirm_password"
            >Password doesn't matched</label
          >
        </div>
      </div>
      <button class="login">Add Device</button>
    </form>
  </div>
  <div v-if="spin_visible" id="spinner">
    <Circle8 class="circle"></Circle8>
  </div>
</template>

<script >
import axios from "axios";
import Swal from "sweetalert2";
import PasswordMeter from 'vue-simple-password-meter';
import Navbar from '../components/Navbar.vue';
import {Circle8} from 'vue-loading-spinner'

export default {
  components: {PasswordMeter,Navbar, Circle8},
  data() {
    return {
      show_meter:false,
      username: "",
      email: "",
      password: "",
      confirm_password: "",
      spin_visible: false,
    };
  },
  methods: {
    submitcredentials() {
     
      //console.log(this.emailvalid, this.email);
      if(!(this.password == this.confirm_password)) {
        this.spin_visible = false;
        return;
      }
      //console.log("here", !this.emailvalid);
      
      axios
        .post("https://codingparina.herokuapp.com/api/registration", {
          name: this.username,
          email: this.email,
          password: this.password,
        })
        .then((response) => {
          this.spin_visible = false;
          this.$router.push({ name: "Login" });

          //console.log(localStorage.getItem("user_id"),response.data.user.id);
        })
        .catch((err) => {
          this.spin_visible = false;
          this.failedsweetalert("Try Again");
        });
    },
    emailavailable() {
      this.spin_visible = true;
      axios
        .get("https://codingparina.herokuapp.com/api/checkmail/" + this.email)
        .then((response) => {
          
          if (response.status == "200") {
              this.spin_visible = false;
              this.failedsweetalert("Email Already Exists");
              
          } 
        })
        .catch((err) => {
          this.submitcredentials();
          //this.$router.push({ name: "Login" });
        });
    },
    
    failedsweetalert(msg) {
      //console.log(localStorage.getItem("islogged"));
      if (localStorage.getItem("isloggedin") == "false") {
        Swal.fire({
          position: "top-end",
          width: 400,
          icon: "error",
          title: "Registration Failed! "+msg,
          showConfirmButton: false,
          timer: 2000,
        });
      }
    },
  },
  created() {},
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

