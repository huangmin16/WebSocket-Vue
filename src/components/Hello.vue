<template>
  <div class="hello" @keydown.13="socket">
    <div class="message">
      <div v-for="ws in wsmsg" :key="ws.from">
        <span v-if="ws.from != name" class="single-left">{{ws.from}}  <span class="single-left-message">{{ws.message}}</span></span>
        <span v-else class="single-right">{{ws.from}}<span class="single-right-message">{{ws.message}}</span></span>
      </div>
    </div>
    <div>
      <input v-model="msg">
      <button @click="socket">发送</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'hello',
  data() {
    return {
      msg: '',
      ws: '',
      wsmsg: [],
      name: '',
      sendName: ''
    }
  },
  methods: {
    socket() {
      let message = {
        "message": this.msg,
        "from": this.name,
      }
      this.ws.send(JSON.stringify(message));
      this.msg = ''
    },
    getMessage(xx) {
      let Message = JSON.parse(xx)
      this.wsmsg.push(Message)
    }
  },
  mounted() {
    this.name = sessionStorage.getItem("name")
    var that = this
    this.ws = new WebSocket("ws://127.0.0.1:3001");//服务端地址
    this.ws.onmessage = function (evt) {
      that.getMessage(evt.data)
    }
  },
  watch: {
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.message {
  display: inline-block;
  min-height: 300px;
  height: 60%;
  width: 80%;
  background-color: #ddd;
}

.single-left {
  display: block;
  position: relative;
  margin-left: 10px;
  margin-top: 5px;
  text-align: left;
}
.single-left-message{
  display: block;
  margin-left: 50px;
}
.single-right {
  display: block;
  position: relative;
  margin-right: 10px;
  margin-top: 5px;
  text-align: right;
}
.single-right-message{
  display: block;
  margin-right: 50px;
}
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
