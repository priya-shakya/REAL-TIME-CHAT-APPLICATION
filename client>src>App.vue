<template>
  <div id="app">
    <ChatComponent />
  </div>
</template>

<script>
import ChatComponent from './components/ChatComponent.vue'

export default {
  components: {
    ChatComponent
  }
}
</script>

<style>
#app {
  font-family: sans-serif;
  text-align: center;
  margin-top: 20px;
}
</style>
