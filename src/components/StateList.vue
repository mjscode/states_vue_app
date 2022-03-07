
<template>
        <h3>{{title}}</h3>
        <ul>
            <div :key="state.id" v-for="state in states">
                <State @state-selected="$emit('state-selected',state.id)" @state-highlighted="$emit('state-highlighted', state.id)" 
                :state = "state" />
            </div>
        </ul>
</template>

<script>
import State from './State.vue'

 export default{
     name: 'StateList',
     props: {
         title:{
             String
         },
         states:{
             Array
         }
     },
     components:{
         State
     }
     }
</script>
<style scoped>
    div{
        padding-top:10px;
    }
</style>