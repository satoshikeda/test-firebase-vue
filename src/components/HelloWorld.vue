<template>
  <div class="hello">
    <h1>TEST Firebase + Vue</h1>
    <input type="text" v-model="data"/>
    <button v-on:click="add">ADD</button>
    <ul>
      <li v-for="item in items">{{ item }}</li>
    </ul>
  </div>
</template>

<script lang="ts">
import * as firebase from "firebase";
import Vue from 'vue';

export default Vue.extend({
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  created() {
    firebase.database().ref('test').on('child_added', this.childAdded)
  },
  data() {
    return {
      data: '',
      items: [''],
    }
  },
  methods: {
    add() {
      firebase.database().ref('test').push({
        test: 'aaa',
        data: this.data
      }, ()=>{
        console.log('add done')
      })
    },
    childAdded(snap: any) {
      console.log('childAdded: ', snap.val())
      const data = snap.val().data
      this.items.push(data)
    }
  }
});
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
