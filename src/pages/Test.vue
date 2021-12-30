<template>
  <h1>Test View だよ</h1>
</template>

<script>
export default {
    data(){
        return{
            test: 'test'
        }
    },
    mounted() {
        console.log(this.test)
    }
}
</script>

<style>

</style>