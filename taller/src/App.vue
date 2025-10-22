<template>
    <router-view></router-view>
</template>

<script>
export default {
  data(){
    return{

    }
  },
  created() {
    this.$router.push('/');
  }
}
</script>

<style>
</style>