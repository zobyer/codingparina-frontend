<template>
  <div class="dropdown">
    <button class="dropbtn">
      <font-awesome-icon class="cart_icon" :icon="['fas', 'user']" />
      <font-awesome-icon :icon="['fas', 'chevron-down']" />
      <h5 >Hello {{user_name}}</h5>
    </button>
    <div class="dropdown-content">
      <button @click="logout" class="drop_button">
        লগ আউট
      </button>
     
    </div>
  </div>

  <div class="add_container">
    <h1>Khoj the search Page</h1>

    <div class="container">
      <div class="box" @click="visibile = false">
        <label><b>Input Values</b></label>
        <input
          type="text"
          placeholder="Enter Input Values .."
          v-model="Inp_val"
          @input="checkvalidity()"
        />
        <label class="error" v-if="Exception_coma"
          >Last Digit Can't be a Comma(,)</label
        >
        <label class="error" v-if="Exception_numbr"
          >You can only write number seperated Comma(,)</label
        >
      </div>

      <div class="box" @click="visibile = false">
        <label><b>Search Value</b></label>
        <input
          type="text"
          placeholder="Enter Search Value .."
          v-model="Search_val"
        />
      </div>
    </div>
    <button class="login" @click="succesfulinsert()">Khoj</button>
    <h1>Result: {{ Search_res }}</h1>
  </div>
</template>

<script >
import axios from "axios";
import Swal from "sweetalert2";
export default {
  components: {},
  data() {
    return {
      user_name:"",
      Inp_val: "",
      Inp_array: [],
      Exception_coma: false,
      Exception_numbr: false,
      Search_val: "",
      Search_res: null,
    };
  },
  methods: {
    succesfulinsert() {
      if (!this.Exception_numbr && !this.Exception_coma) {
        this.Inp_array = this.Inp_val.split(",");
        this.Inp_array.sort(function (a, b) {
          return b - a;
        });

        this.Search_res = this.binarySearch(this.Inp_array, this.Search_val);
        this.callinsertapi();
        this.clearinput();
      }
      //console.log(this.Search_res, this.Inp_array, this.Search_val);
    },
    callinsertapi() {
      console.log(localStorage.getItem("user_id"));
      axios
        .post(
          "https://codingparina.herokuapp.com/api/insertinputs",
          {
            user_id: localStorage.getItem("user_id"),
            input_values: this.Inp_array.toString(),
          },
          {
            headers: {
              Authorization: "Bearer " + localStorage.getItem("token"),
            },
          }
        )
        .then((response) => {
          this.sweetalert();
          //console.log(response);
        })
        .catch((err) => {
          this.failedsweetalert();
          //console.log(err);
        });
    },

    checkvalidity() {
      if (!/^([+-]?[0-9]+,?)+$/.test(this.Inp_val)) {
        this.Exception_numbr = true;
        this.Exception_coma = false;
      } else if (this.Inp_val[this.Inp_val.length - 1] == ",") {
        this.Exception_coma = true;
        this.Exception_numbr = false;
      } else {
        this.Exception_numbr = false;
        this.Exception_coma = false;
      }
    },

    binarySearch(arr, x) {
      let start = 0,
        end = this.Inp_array.length - 1;
      while (start <= end) {
        let mid = Math.floor((start + end) / 2);
        // console.log(
        //   parseInt(arr[mid]),
        //   parseInt(x),
        //   parseInt(arr[mid]) < parseInt(x)
        // );
        if (arr[mid] === x) return true;
        else if (parseInt(arr[mid]) > parseInt(x)) start = mid + 1;
        else end = mid - 1;
      }

      return false;
    },
    clearinput() {
      this.Inp_val = "";
      this.Search_val = "";
    },
    logout() {
      localStorage.clear();
      localStorage.setItem("isloggedin", false);
      this.$router.push({ name: "Login" });
    },
    failedsweetalert() {
      //console.log(localStorage.getItem("islogged"));
      if (localStorage.getItem("isloggedin") == "false") {
        Swal.fire({
          position: "top-end",
          width: 400,
          icon: "error",
          title: "Oops! Failed to insert",
          showConfirmButton: false,
          timer: 2000,
        });
      }
    },
    sweetalert() {
      //console.log(localStorage.getItem("islogged"));
      if (localStorage.getItem("isloggedin") == "true") {
        Swal.fire({
          position: "top-end",
          width: 400,
          icon: "success",
          title: "Data inserted successfully",
          showConfirmButton: false,
          timer: 1500,
        });
      }
    },
  },
  created(){
    if( localStorage.getItem("isloggedin") == "true"){
      this.user_name = localStorage.getItem("user_name");
    }else{
      this.$router.push({ name: "Login" });
    }
  }
};
</script>

<style>

.dropdown {
  float: center;
  margin-bottom: 50px;
}
.dropdown .dropbtn {
  font-size: 17px;
  border: none;
  outline: none;
  color: #c1436d;
  padding: 14px 16px;
  background-color: inherit;
  font-family: inherit;
  margin: 0;
  
}
.dropdown-content {
  display: none;
  position: absolute;
  background-color: #f9f9f9;
  min-width: 160px;
  box-shadow: 0px 8px 16px 0px rgba(0, 0, 0, 0.2);
  z-index: 1;
  left: 45%;
}
.dropdown-content a,
.drop_button {
  float: none;
  color: #c1436d;
  padding: 12px 16px;
  text-decoration: none;
  display: block;
  text-align: left;
  cursor: pointer;
}
.dropbtn:hover,
.drop_button:hover, .dropbtn h5:hover{
  background-color: #c1436d;
  color: white;
  border-radius: 25%;
  transition: 0.4s ease;
}
.dropdown a:hover {
  background: white;
  border-radius: 0;
  border-radius: 25%;
}
.dropdown-content a:hover {
  background-color: #c1436d;
  color: white;
}
.dropdown:hover .dropdown-content {
  display: block;
}
.dropbtn h5 {
  color: #000;
  margin: 0;
}
.drop_button {
  background: none;
  margin: auto;
  border: none;
  font-size: 17px;
}


.add_container {
  margin: auto;
  width: 50%;
  font-family: "Open Sans", sans-serif;
}
.add_container .container input[type="text"] {
  color: black;
}

.box {
  margin: 15px;
}
.error {
  color: #ff0057;
}
@media screen and (max-width: 800px) {
  .add_container {
    width: 70%;
  }
}
</style>