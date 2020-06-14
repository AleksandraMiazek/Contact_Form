<template>
<div>
  <h1>hello vue</h1>

  <FormValidation/> <!--  @add="add2" -->
</div>
</template>

<script>
import FormValidation from './components/FormValidation.vue'
export default {
  name: 'App',
 components: {
   FormValidation
 },
 
 }
</script>

<style>

</style>
