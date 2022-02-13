<template>
  <div id="app">
    <img
      src="https://media2.giphy.com/media/12imXZa2uBqf28/giphy.gif?cid=ecf05e47jd7lx1kp5zbgmrgnpg6jx5pr40n15otrbuhql8yp&rid=giphy.gif&ct=g"
      alt="Fun Giphy"
    />
    <h1>lines Gallore!</h1>
    <form ref="line_form" action="javascript:void(0)">
      <input ref="line_content" type="text" placeholder="Content" />
      <input @click="post_line" type="submit" value="Submit" />
    </form>
    <h3>{{ post_status }}</h3>
    <div class="line_container">
      <div v-for="line in lines" :key="line[2]" class="line_card">
        <p class="line_content">{{ line[0] }}</p>
        <h6 class="line_desc">{{ line[1] }}</h6>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      lines: [],
      post_status: "",
    };
  },
  // Make sure you pay attention to the URL being used! Put that variable in the .env.local file!
  // Also make sure you change that after you do your local testing!
  methods: {
    post_line() {
      this.post_status = "Posting New line!";
      axios
        .request({
          url: `${process.env.VUE_APP_API_URL}/api/line`,
          method: "POST",
          data: {
            content: this.$refs["line_content"].value,
          },
        })
        .then((res) => {
          res;
          this.$refs["line_form"].reset();
          this.post_status = "line Created!";
        })
        .catch((err) => {
          console.log(err);
          this.post_status = "Error Posting New line, Sorry!";
        });
    },
  },
  mounted() {
    this.post_status = "Loading Lines";
    axios
      .request({
        url: `${process.env.VUE_APP_API_URL}/api/line`,
        method: "GET",
      })
      .then((res) => {
        console.log(res);
        this.lines = res.data;
        this.post_status = "";
      })
      .catch((err) => {
        console.log(err);
        this.post_status = "Error Loading lines, Sorry!";
      });
  },
};
</script>

<style>
img {
  max-width: 100%;
  object-fit: cover;
}

input[type="text"],
input[type="text"]:focus,
input[type="text"]:active {
  border: none;
  border-bottom: 2px solid black;
  margin: 10px;
  outline: none;
}

input[type="submit"]:hover {
  cursor: pointer;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

form {
  display: grid;
  place-items: center;
  width: 50%;
}

.line_container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  column-gap: 50px;
  row-gap: 50px;
  width: 90%;
  margin-left: 5%;
  place-items: center;
  margin-top: 50px;
}

.line_card {
  box-shadow: 3px 3px 6px grey;
  border-radius: 15px;
  transition: all 0.25s ease-in-out;
  max-width: 400px;
  padding: 10px;
}

.line_card:hover {
  box-shadow: 6px 6px 9px grey;
}

.line_card:focus,
.line_card:active {
  box-shadow: 1px 1px 2px grey;
}
</style>
