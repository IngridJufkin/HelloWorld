<template>

<div class="bg-white shadow rounded px-3 pt-3 pb-5 border border-white">

    <div>
      <h2> Pick a date </h2>
     <div>
        <input
        v-model="input.year"
         placeholder ="Year"
         min="2000"
         max="2030"
         step="1"
         type="number"
         class="mt-1 block w-full border-gray p-1"/>
      </div><br>

      <div>
        <input
        v-model="input.month"
         placeholder ="Month"
         min="1"
         max="12"
         step="1"
         type="number"
         class="mt-1 block w-full border-gray p-1"/>
      </div><br>
      
      <div>
        <input
        v-model="input.day"
         placeholder ="Day"
         type="number"
         min="1"
         max="31"
         step="1"
         class="mt-1 block w-full border-gray p-1"/>
    </div>
    <br>

    {{input.month}}
    <div class="row mt-5">
      <div class="col text-right">
        <button
        @click="getAndFormatDate()"
        class="bg-green-400 px-4 py-2 rounded">
        Add date
        </button>
      </div>
    </div>
  </div>
  


<div v-if ="items"> 
<div v-for="(item, index) in items">
<div>
<h3>
<a v-bind:href="item.content_urls.desktop.page">{{ index + 1  }}. {{ item.titles.normalized }} </a>
</h3>
</div>
<div>
{{ item.description }}
</div >
<div v-if ="item.thumbnail">
 <img :src="item.thumbnail.source"/>
</div>
</div>
<!-- 

  <li v-for="item in items" :key="item">
   {{ items.mostread }} -->
    <!-- {{ items.displaytitle }}  -->
<!-- {{ items.description }} -->
    <!-- {{ items.tfa.mostread}} -->
    
  </li>
  </div>
  
</template>

<script>
import axios from 'axios';
export default {
  name: 'HelloWorld',
    data () {
    return {
      loading: true,
      errored: false,
       items:[],
       thumbnail:[],
      input: {
        year: null,
        month: null,
        day: null,
        date: null,
      }

    }
    
  },

  props: {
    msg: String
  },

  methods: {
    getAndFormatDate() {
    let day = this.input.day;
    let month = this.input.month;
    let year = this.input.year;

    let date = year + "/" + month + "/" + day
    if (month < 10) {
          date = year + "/0" + month + "/" + day
    }
    if (day < 10) {
          date = year + "/" + month + "/0" + day
    }
    if (month < 10 && day < 10) {
          date = year + "/0" + month + "/0" + day
    }
    this.input.date = date
  console.log(date)

  axios
      .get(`https://en.wikipedia.org/api/rest_v1/feed/featured/${this.input.date}`)
      //.get(`https://en.wikipedia.org/api/rest_v1/feed/featured/2021/04/30`)
      .then(response => {
        console.log (response.data)
        this.items = response.data.mostread.articles
         this.thumbnail = response.data.mostread.articles.thumbnail
         console.log("olen siin" + this.input.date),
       
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
      },

 }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

h2 {
  margin: 10px 0px;
}

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
