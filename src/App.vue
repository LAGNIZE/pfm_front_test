<template>
  <div class="container">
    <HelloWorld msg="Welcome to Vue.js + TypeScript App" />
    <user-list
      v-bind:users="users"
      v-on:modal="toggleModal"
      v-bind:modalData="modalData"
    ></user-list>
    <div class="btns">
      <a @click="fetchData" class="waves-effect waves-light btn">Fetch Data</a>
      <a @click="saveUsers" class="waves-effect waves-light btn">Save Users</a>
      <a @click="removeUsers" class="waves-effect waves-light btn"
        >Remove Users</a
      >
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import HelloWorld from "./components/HelloWorld.vue";
import axios from "axios";
import { UserTypes } from "./types/UserTypes";
import UserList from "./components/UserList.vue";
import M from "materialize-css";

export default defineComponent({
  name: "App",
  data() {
    return {
      users: {
        data: {},
      } as UserTypes,
      modalData: {
        modalName: "",
        modalSurname: "",
        modalEmail: "",
      },
    };
  },
  components: {
    HelloWorld,
    UserList,
  },
  mounted() {
    if (localStorage.getItem("users")) {
      try {
        this.users = JSON.parse(localStorage.getItem("users") || "{}");
      } catch (e) {
        localStorage.removeItem("users");
      }
    }
  },
  methods: {
    async fetchData(): Promise<void> {
      try {
        await axios
          .get<UserTypes>("https://reqres.in/api/users?page=2")
          .then((response) => {
            this.users.data = response.data.data;
          });
      } catch (e) {
        alert(e);
      }
    },
    saveUsers(): void {
      const parsed: string = JSON.stringify(this.users);
      localStorage.setItem("users", parsed);
    },
    removeUsers(): void {
      localStorage.removeItem("users");
    },
    toggleModal(user: {
      first_name: string;
      last_name: string;
      email: string;
    }): void {
      M.AutoInit();
      this.modalData.modalName = user.first_name;
      this.modalData.modalSurname = user.last_name;
      this.modalData.modalEmail = user.email;
    },
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.btns .btn:not(:last-child) {
  margin-right: 30px;
}
</style>
