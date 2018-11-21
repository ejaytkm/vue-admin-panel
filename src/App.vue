<template>
  <div id="app">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <div style="padding: 50px">
      <simple-table
        :inputdata="testingData"
        :configdata="config"
      />
    </div>
  </div>
</template>

<script>
import simpleTable from './components/vue-simple-table.vue'

export default {
  name: 'app',

  components: {
    simpleTable
  },

  data () {
    return {
      testingData: [
        {
          firstname: 'Ejayarin',
          lastname: 'Tamarin',
          address: 'Mona Ciara, Perole Jansen',
          age: '52',
          dob: '1993-06-14',
          created: '2018-06-14T12:00:55Z'
        }, {
          firstname: 'Calvineese',
          lastname: 'Ongela',
          address: 'St Boloh, Richardson Road',
          age: '42',
          dob: '1994-10-10',
          created: '2018-06-16T12:44:00Z'
        }
      ],

      config: {
        filename: 'mytesting.com'
      }
    }
  },

  created () {
    // lazy duplication of data
    for (let i = 0; i < 5; i++) {
      this.testingData.map(data =>{
        this.testingData.push(data)
      })
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
