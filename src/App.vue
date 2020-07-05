<template>
  <div id="app">
    <nav class="navbar navbar-dark bg-primary">
      <span class="navbar-brand mb-0 h1">Report Of The Machine Statuses</span>
    </nav>
    <div class="container-fluid">
      <Loader v-if="loading" />
      <div v-if="!loading" class="machine-list">
        <Report 
          v-for="machine in as_A" :key="machine.MACHINE"
          :machine="machine"
          :color="as_B[machine.MACHINE]" 
          :ooe="as_C.find(el => el.MACHINE === machine.MACHINE)" 
          />
      </div>
      
    </div>
    
  </div>
</template>

<script>
import Loader from './components/Loader'
import axios from 'axios'
import Report from './components/Report'

export default {
  name: 'App',
  components: {
    Loader,
    Report
  },
  data () { return {
    loading: true,
    as_A: [],
    as_B: [],
    as_C: []
    }
  },
  created () {
    
    const requestOne = axios.get('https://www.marviq.com/assessment/index.php?page=a&from=2018-01-07%2000:00:00');
    const requestTwo = axios.get('https://www.marviq.com/assessment/index.php?page=b&from=2018-01-07%2000:00:00');
    const requestThree = axios.get('https://www.marviq.com/assessment/index.php?page=c&from=2018-01-07%2000:00:00');
    axios.all([requestOne, requestTwo, requestThree])
      .then(axios.spread((...responces) => {
          this.as_A = responces[0].data
          this.as_B = responces[1].data
          this.as_C = responces[2].data
          console.log(responces)
          this.loading = false
      }))
      .catch(error => console.log(error))
  }

}
</script>

<style>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
