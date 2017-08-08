<template>
  <div class="hello">
    <input v-model="name"><button @click="login">登录</button>
  </div>
</template>
<script>
export default {
  name: 'hello',
  data() {
    return {
        name:"",
        password:''
    }
  },
  methods: {
    login(){
        sessionStorage.setItem("name",this.name ) 
        this.$router.push("/hello")
    }
  },
  mounted() {
   
  },
  watch:{
  }

}
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
</style>