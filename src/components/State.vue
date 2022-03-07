<template>
    <li @click="$emit('state-selected',state.id)" @dblclick="$emit('state-highlighted',state.id)"
     :class= "[state.highlighted? 'highlighted' : '']">
        {{state.state}}
    </li>
</template>
<script>
export default {
    name: 'State',
    props:{
        state:{
            String
        }
    }
}
</script>
<style scoped>
    li{
        cursor: pointer;
        user-select: none;
    }
    .highlighted{
        color: blue !important;
    }
</style>