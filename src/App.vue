<template>
  <div id="app" class="container">
    <ul class="list-group">
      <li class="list-group-item" v-for=" item in items">Cras : {{item}}</li>
    </ul>

    <pagination :page.sync="page" :page-size.sync="size" :total.sync="total" :max-link.sync="maxLink" :event-name="eventName"></pagination>

  </div>
</template>

<script>
  import pagination from './components/pagination'

  export default {
    data(){
      return {
        page:1,
        size:20,
        total:100,
        maxLink:5,
        eventName:'go'
      }
    },
    components: {
      pagination
    },
    computed:{
      items(){
        let arrays = [];
        for (let i = (this.page-1) * this.size;i<(this.page) * this.size;i++){
          arrays.push(i);
        }
        return arrays
      }
    },
    events: {
      'go': function (page) {
          console.log("page:"+page)
      }
    }
  }
</script>

<style>
  html {
    height: 100%;
  }

  body {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 100%;
  }

  #app {
    color: #2c3e50;
    margin-top: -100px;
    max-width: 600px;
    font-family: Source Sans Pro, Helvetica, sans-serif;
    text-align: center;
  }

  #app a {
    color: #42b983;
    text-decoration: none;
  }
</style>
