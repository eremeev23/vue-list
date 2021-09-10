<template>
   <div class="app">
     <List :messages__data__first = 'messages' 
           :messages__data__rest = 'messagesRest'
           :messages__data__all = 'messagesAll'/>
   </div>
</template>

<script>
import List from "./components/List"

export default {
  name: 'app',
  components: {
    List,
  },
  data() {
    return{
      messages: [],
      messagesAll: [],
      messagesRest: []
    }
  },
  created() {
      fetch('http://localhost:3001/data')
      .then(response =>  response.json())
      .then(json => this.messages = json.slice(0, 10));

      fetch('http://localhost:3001/data')
      .then(response =>  response.json())
      .then(json => this.messagesAll = json);

      fetch('http://localhost:3001/data')
      .then(response =>  response.json())
      .then(json => this.messagesRest = json.slice(10, -2));
  },
}
</script>
  
<style>
#app {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #6f7575;
}
</style>
