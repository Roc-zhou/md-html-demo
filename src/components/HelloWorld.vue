<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <div class="editor">
      <mavon-editor v-model="value" @change="changeMd" :ishljs = "true"/>
    </div>
  </div>
</template>

<script>
import { mavonEditor } from 'mavon-editor'
import 'mavon-editor/dist/css/index.css'
export default {
  name: 'HelloWorld',
  components: {
      mavonEditor
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      value:''
    }
  },
  methods:{
    changeMd(v,r) {
      console.log(r);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
