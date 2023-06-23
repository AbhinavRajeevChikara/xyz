<template>
  <base-card>
    <div class="container1">
      <form @submit.prevent="loginData">
        <div class="form-group">
          <div class="center">
            <label for="email" class="emaillabel">Email:</label><br />
          </div>
          <input
            class="form-control"
            type="email"
            name="email"
            placeholder="email@email.com"
            id="Input"
            required
            v-model="data.email"
          /><br /><br />
        </div>
        <div class="form-group">
          <label for="password" class="passwordlabel">Password:</label><br />
          <input
            class="form-control"
            type="password"
            name="password"
            placeholder="*********"
            id="Input"
            required
            v-model="data.password"
          />
        </div>
        <br />
        <button class="btn btn-large btn-success" type="submit">
          <font-awesome-icon icon="fa-solid fa-user" class="font1" /> LOGIN
        </button>
        <br />
        <router-link to="/forget" class="nav-link" id="forgot"
          >Forgot Password?</router-link
        >
      </form>
      <!-- <button @click="openPopup">hhhh</button> -->
    </div>
  </base-card>
  <div v-if="showPopup" class="popup">
    <div class="popup-content">
      <h2 style="color: rgb(251, 6, 6)">Welcome to the Home Page</h2>
      <!-- <p>This is an example of a good quality popup using Vue.js.</p> -->
    </div>
  </div>
  <div v-if="showPopup2" class="popup">
    <div class="popup-content">
      <h2 style="color: rgb(251, 6, 6)">You no enter to the Home Page</h2>
      <!-- <p>This is an example of a good quality popup using Vue.js.</p> -->
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      data: {
        password: "",
        email: "",
      },
      showPopup: false,
      showPopup2: false,
    };
  },
  methods: {
    loginData() {
      axios
        .post("http://127.0.0.1:8000/api/login", {
          email: this.data.email,
          password: this.data.password,
        })
        .then((response) => {
          if (response.status === 200) {
            // Assuming the API response includes an authentication token
            const authToken = response.data.token;

            // Save the authentication token in localStorage or Vuex store
            localStorage.setItem("authToken", authToken);
            // Store the data in vuex for use in hover
            this.$store.commit("setAPIResponse", response.data);
            // Redirect to the home page
            this.openPopup();
            setTimeout(() => {
              this.$router.push("/home");
            }, 3000);
          } else {
            // alert("Your email and password are incorrect.");
            console.log("Login Successfully", response);
          }
        })
        .catch((error) => {
          this.openPopup2();
          setTimeout(() => {
            // this.$router.push("/home");
          }, 3000);
          console.error("Login failed", error);
        });

      this.clearForm();
    },
    clearForm() {
      this.data.email = "";
      this.data.password = "";
    },
    // for the first popup( when your email or password is correct)
    openPopup() {
      this.showPopup = true;
      setTimeout(this.closePopup, 3000);
    },
    closePopup() {
      this.showPopup = false;
    },
    //for the second popup( when your email or password is incorrect)
    openPopup2() {
      this.showPopup2 = true;
      setTimeout(this.closePopup2, 3000);
    },
    closePopup2() {
      this.showPopup2 = false;
    },
  },
};
</script>
<style scoped>
form {
  display: block;
  margin-top: 0em;
  width: 100%;
}
.container1 {
  justify-content: center;
  justify-items: start;
  background-color: beige;
  width: 100% !important;
  height: 100%;
  display: flex;
  align-items: center;
  width: fit-content;
  padding-right: 1rem;
  padding-left: 1rem;
  text-align: -webkit-center;
  position: relative;
}
#Input {
  width: 30rem;
  height: 2rem;
  font-size: 1rem;
}
.emaillabel {
  font-size: 2rem;
}
.passwordlabel {
  font-size: 2rem;
}
#forgot {
  margin-top: 1%;
  margin-left: 26%;
}
.popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(251, 248, 248, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.popup-content {
  margin-top: 35%;
  margin-left: 66%;
  background-color: #fcf9f9;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
  max-width: 500px;
  text-align: center;
}

.popup h2 {
  margin-top: 0;
}

.popup button {
  padding: 10px 20px;
  background-color: #4caf50;
  color: #fff;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

.popup button:hover {
  background-color: #45a049;
}
</style>
