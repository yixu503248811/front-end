<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div>
      <p>Male</p>
      <ul class="menu-list">
        <li v-for="item in maleCatList" :key="item">{{ item }}</li>
      </ul>
      <p>Female</p>
      <ul class="menu-list">
        <li v-for="item in feMaleCatList" :key="item">{{ item }}</li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",
  data() {
    return {
      maleCatList: [],
      feMaleCatList: [],
      msg: "Welcome to my front-end part"
    };
  },
  created: function() {
    this.getItems();
  },
  methods: {
    getItems: function() {
      axios
        .get("http://5c92dbfae7b1a00014078e61.mockapi.io/owners")
        .then(response => this.getList(response.data))
        .catch(function(error) {
          // 请求失败处理
          console.log(error);
        });
    },
    getList: function(event) {
      for (var i = 0; i < event.length; i++) {
        if (event[i].gender === "Male" && event[i].pets !== null) {
          for (var j = 0; j < event[i].pets.length; j++) {
            if (event[i].pets[j].type === "Cat") {
              if (this.maleCatList.indexOf(event[i].name) === -1) {
                this.maleCatList.push(event[i].name);
              }
            }
          }
          this.maleCatList.sort();
        } else if (event[i].gender === "Female" && event[i].pets !== null) {
          for (var j = 0; j < event[i].pets.length; j++) {
            if (event[i].pets[j].type === "Cat") {
              if (this.feMaleCatList.indexOf(event[i].name) === -1) {
                this.feMaleCatList.push(event[i].name);
              }
            }
          }
          this.feMaleCatList.sort();
        }
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.menu-list {
  margin-left: 200px;
}
.menu-list li {
  clear: both;
  width: 100%;
  margin: 10px 0;
}
</style>
