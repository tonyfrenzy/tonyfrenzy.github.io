<template>
  <div id="app">
    <HeaderDiv/>

    <NominateDiv/>

    <FooterDiv/>
  </div>
</template>

<script>
import HeaderDiv from './components/HeaderDiv.vue'
import NominateDiv from './components/NominateDiv.vue'
import FooterDiv from './components/FooterDiv.vue'

export default {
  name: 'App',
  components: {
    HeaderDiv,
    NominateDiv,
    FooterDiv
  }
}
</script>

<style>

</style>
