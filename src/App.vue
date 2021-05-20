<template>
  
  <router-view/>
</template>

<script>
import Swal from "sweetalert2";


export default {
  components:{},
  data() {
    return {
      spin_visible:false,
    };
  },
  methods: {
    sweetalert(msg) {
      //console.log(localStorage.getItem("islogged"));
      if (localStorage.getItem("isloggedin") == "true") {
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
    $route(to, from) {
      
      if (to.name == "Home" && from.name == "Login") {
        this.sweetalert("You have successfully logged in!");
      }
    },
    
  },
};
</script>

<style >
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#spinner{
  position: absolute;
  background: white;
  top: 50%;
  right:50%;
  size: 600px;
}


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
  font-family: "Open Sans", sans-serif;
}
.login:hover {
  background: #ff0057;
  color: white;
  transition: 0.5s;
}

.swal2-title{
  font-family: 'Open Sans', sans-serif;
  font-size: 20px !important;
}
.swal2-popup {
font-size: .5rem !important;
}
</style>