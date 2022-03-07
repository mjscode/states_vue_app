<template>
    <div>
        <h3>State Details</h3>
        <div v-if="state.state">
            <h4>State Name: {{state.state}}</h4>
            <p>State Population: {{state.population}}</p>
            <p>Number of Counties: {{stateCounties.length}}</p>
            <h4>Counties: </h4>
            <ul>
                <li :key="county.county" v-for="county in stateCounties">
                    <p>Name: {{county.county}}</p>
                    <p>Population: {{county.population}}</p>
                </li>
            </ul>
            <p>Total Counties Population: {{this.totalCountiesPopulation()}}</p>
            <h4 v-if="this.populationMatch()" class="match">Total Counties Population matches State Population.</h4>
            <h4 v-else class="not-match">Total Counties Population does not match State Population.</h4>
        </div>
        <div v-else>
            <h4>No state selected</h4>
        </div>
    </div>
</template>
<script>
export default {
    name: 'StateDetails',
    props:{
        state:{
            Object
        },
        stateCounties:{
            Array
        }
    },
    methods:{
        totalCountiesPopulation(){
            return this.stateCounties.reduce((a,b) => a + b.population, 0)
        },
        populationMatch(){
            return this.totalCountiesPopulation() === this.state.population;
        }
    }
}
</script>
<style >
    .match {
        color:green !important;
    }
    .not-match{
        color: red !important;
    }
</style>