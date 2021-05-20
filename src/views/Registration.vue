<template>
  <navbar />
  <!-- all elements become invisible when vue loading starts -->
  <div v-if="!spin_visible" class="add_container">
    <h1>Registration</h1>
    <!-- form calls the emailavailable function after each submission -->
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
          <!-- when user starts writing password it shows the strength of password through different color -->
          <div class="pass_meter">
            <label v-if="show_meter" class="error">Password Strength Meter </label>
            <!-- shows the password strength -->
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
          <!-- Shows error message if both password does not matches -->
          <label class="error" v-if="password != confirm_password"
            >Password doesn't matched</label
          >
        </div>
      </div>
      <button class="login">Registration</button>
    </form>
  </div>
  <!-- vue loading becomes visible based on spin visible variable -->
  <div v-if="spin_visible" id="spinner">
    <Circle8 class="circle"></Circle8>
  </div>
</template>

<script >
import axios from "axios";   // to all api 
import Swal from "sweetalert2";  // for showing pop up message
import PasswordMeter from 'vue-simple-password-meter'; // password strength
import Navbar from '../components/Navbar.vue';  // vue componet contains the navigation bar
import {Circle8} from 'vue-loading-spinner' // vue loading spinner

export default {
  components: {PasswordMeter,Navbar, Circle8},
  data() {
    return {
      show_meter:false,  // shows the message of password meter
      username: "",      // username
      email: "",         // Registration
      password: "",      // password
      confirm_password: "",  // confirming the password
      spin_visible: false,   // shows the vue loading spinner
    };
  },
  methods: {
    // function send registration credentials to the backend api for storing the the server
    // first it check if both the password matches. for different password function returns
    // without further proceding and set the vue loader visibility to off and other element on
    // if password matches then call the post api method to store the data
    // for successfull insertion page redirect to login page and also stops the vue loader visibility off

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

    // function: calls when user submit registration form
    // set vue loader visibility on and other element of the web page to off
    // call api to check if the email already exists
    // for duplicate email calls failedsweetlaert method with a parameter 
    // failedsweetalert method parameter is the message for the pop up notification
    // if email does not exist then calls submitcredential function

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

    // function: shows popup notifications
    // parameter display error message
    
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

