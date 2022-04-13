<template>
<v-container>
  <v-row justify="center">  
    <h1>{{propsdata.length > 0 ? propsdata.length + "개의 할일이 남아있어요!" : "한가하시네요!"}}</h1>  
  </v-row>
</v-container>
</template>

<script>
export default {
  props: ["propsdata"],
}
</script>

<style scoped>
  h1 {
    color: #2F3B52;
    font-weight: 900;
    margin: 2.5rem 0 1.5rem;
  }
</style>
