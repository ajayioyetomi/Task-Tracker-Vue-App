<template>
    <button @click="onClick()" :style="{backgroundColor:color}" class="btn">{{text}}</button>
</template>

<script>
    export default{
        name:'Button',
        props:{
            text:String,
            color:String
        },
        methods:{
            onClick(){
                console.log('hello')
            }
        }
    }
</script>

<style scoped>

</style>