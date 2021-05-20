<template>
  
  <router-view/>
</template>

<script>
import Swal from "sweetalert2";


export default {
  components:{},
  data() {
    return {
    };
  },
  methods: {
    // function: shows popup notification after successful log in
    // conditions: user mus have to be logged in
    sweetalert(msg) {
      //console.log(localStorage.getItem("islogged"));
      if (localStorage.getItem("isloggedin") == "true") {   // log in checking
        Swal.fire({
          position: "top-end",
          width: 400,
          icon: "success",
          title: msg,
          showConfirmButton: false,
          timer: 1500,
        });
      }
    },
  },
  watch: {
    // fucntion: detects the route change from login to home page
    // calls the swleetarelt funtion for popup notification
    $route(to, from) {
      
      if (to.name == "Home" && from.name == "Login") {
        this.sweetalert("You have successfully logged in!");
      }
    },
    
  },
};
</script>

<style >
/* basic style for all whole page */
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

/* vue loading spiner style to show on the center  */

#spinner{
  position: absolute;
  background: white;
  top: 50%;
  right:50%;
}

/* common style for all the input fields */

input[type="password"],input[type="email"],
input[type="text"] {
  width: 100%;
  padding: 12px 20px;
  margin: 15px 0;
  display: inline-block;
  background: none;
  color: black;
  outline: none;
  border: none;
  border-bottom: 2px solid #ff0057;
  box-sizing: border-box;
  font-family: "Open Sans", sans-serif;
  font-size: 18px;
}

/* style for button */

.login {
  background-color: #04aa6d;
  color: white;
  padding: 10px 15px;
  margin: 8px 0;
  border: none;
  cursor: pointer;
  width: 40%;
  border-radius: 20px;
  font-size: 20px;
}

/* button hover */
.login:hover {
  background: #ff0057;
  color: white;
  transition: 0.5s;
}

/*sweet alert style */

.swal2-title{
  font-family: 'Open Sans', sans-serif;
  font-size: 20px !important;
}
.swal2-popup {
font-size: .5rem !important;
}
</style>