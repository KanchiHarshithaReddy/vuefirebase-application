<template>
  <div>
    <div class="container"></div>
    <div class="card">
      <form @submit.prevent="onFormSubmit">
        <label for="company">Company Name : </label><br>
        <input
          type="text"
          id="company"
          placeholder="Enter Your company name .."
          v-model="user.CompanyName"
        />
        <br /><br />
        <label for="email">Email : </label><br>
        <input
          type="email"
          id="email"
          placeholder="Enter Your email.."
          v-model="user.email"
        />
        <br /><br />
        <label for="state">State : </label><br>
        <input
          type="text"
          id="state"
          placeholder="Enter Your state.."
          v-model="user.state"
        />
        <br /><br />

        <input type="submit" value="Submit" v-on:click="onFormSubmit" class="btn btn-info" />
      </form>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  name: "AddData",
  data() {
    return {
      user: {},
    };
  },
  methods: {
    onFormSubmit(e) {
      e.preventDefault();
      console.log(this.user);
      const result = axios.post(
        "https://vuefirebasetask-default-rtdb.firebaseio.com/post.json",
        this.user
      );
      this.user = result.data;
    },
  },
};
</script>
<style >
.card {
  height: 400px;
  width: 600px;
  margin-left: auto;
  margin-right: auto;
  padding-top: 10px;
  border: 2px solid black;
}
.card input{
  height: 40px;
  width: 450px;
}

</style>