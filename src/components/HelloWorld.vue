<template>
<div v-if ="res"> 
  <h1> {{res.titles.normalized}}</h1>
  <span>{{res.description}} </span>
  <img v-bind:src="res.thumbnail.source" />
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HelloWorld',
    data () {
    return {
      res: null,
      loading: true,
      errored: false
    }
  },
  props: {
    msg: String
  },
mounted () {
    axios
      .get('https://en.wikipedia.org/api/rest_v1/page/summary/Tartu')
      .then(response => {
        console.log (response.data)
        this.res = response.data;
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
  }
}
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
