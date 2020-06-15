<template>
<div>
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
body
{
	/* Background pattern from subtlepatterns.com */
  background-image: url("../public/hexellence.png");
}
</style>
