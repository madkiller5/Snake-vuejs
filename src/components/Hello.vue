<template>
  <div>
    <p>{{ title }}</p>
  </div>

</template>

<script>
export default {
  data () {
    return {
        title: 'Welcome in Snake game!'
    }
  }
}
</script>

<style scoped>
p{
    font-size: 40px;
    font-family: 'Caveat', cursive;
    margin: 20px 0 20px 0;
}
</style>
