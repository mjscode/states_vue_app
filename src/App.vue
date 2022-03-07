<template>
  <header >
    <h1>The States of U.S.A.</h1>
  </header>
  <main>
  <div id ="input-bar">
    <div class="input-divs" id="input-offset"></div>
    <div class="input-divs" id="input-div">
      <input type="text" v-model="filterValue" @input="filterStates" placeholder="Search State">
    </div>
  </div>
  <div id="display-main">
    <div class="display-block">
      <StateList @state-selected="stateSelected" @state-highlighted="highlightState" :states ="states" title="State List"/>
    </div>
    <div class="display-block">
        <StateDetails :state="selectedState" :stateCounties="stateCounties"/>
    </div>
    <div class="display-block">
      <StateList @state-selected="stateSelected" @state-highlighted="highlightState" 
      title="Filtered States" :states="filteredStates" />
    </div>
  </div>
  </main>
</template>

<script>
import StateList from './components/StateList.vue'
import StateDetails from './components/StateDetails.vue'

export default {
  name: 'App',
  components:{
    StateList,
    StateDetails
  },
  methods:{
    stateSelected(id){
      if(this.selectedState.id === id){
        return;
      }
      this.states.map( state => {
        return state.highlighted = false;
      })
      let selectedState = this.states.find(state => {return state.id === id})
      fetch(selectedState.detail)
      .then(res => res.json())
      .then( response => {
        this.selectedState = selectedState;
        this.stateCounties = response.data;
      })
      .catch( e => {
        alert("Sorry something went wrong, whe were not able to get state detail.")
      })
    },
    filterStates(){
      let filterValue = new RegExp(this.filterValue,'gi');
      this.filteredStates = this.states.filter(state =>{
         return state.state.match(filterValue);
      })
    },
    highlightState(id){
      this.states = this.states.map(state=>{
        if(state.id === id){
          state.highlighted = !state.highlighted;
        }
        return state;
      })
      this.filterStates();
    }
  },
  data(){
    return{
      states : [],
      selectedState : {},
      stateCounties : [],
      filterValue: '',
      filteredStates : []
    }
  },
  created(){
    fetch("http://pos.idtretailsolutions.com/countytest/states")
    .then(res => res.json())
    .then( response => {
      let states = response.data;
      states.map((state,index)=>{
        state.id = index+1;
        state.highlighted = false;
        return state;
      })
      this.states = states;
      this.filteredStates = states;
    }).catch(e=>{
      alert("Sorry something went wrong, we were not able to get the states.")
    })
  }
}
</script>

<style>
header{
  text-align: center;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.display-block{
  display: inline-block;
  width: 20%;
  margin: 1% 5%;
  height: 500px;
  max-height:500px;
  width: 225px;
  border: 1px solid black;
  overflow:scroll;
  overflow-x: hidden;
}
.input-divs{
  display: inline-block;
}
#input-offset{
  width:50%;
}
.input-bar{
  width:100%;
}

</style>
