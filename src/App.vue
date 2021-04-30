<template>
  <div id="app">
      <Form v-if="!this.validated"/>
   
    <div class="success-wrapper" v-if="validated">
      <Success />
    </div>

  </div>
</template>

<script>

import Form from './components/Form.vue'
import Success from './components/Success.vue'
export default {
  name: 'Page',
  components: {
    Form,
    Success

  },
  data(){
    return{
      validated: false
    }
  }
}
</script>

<style lang='sass'>


</style>
