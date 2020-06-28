<template>
  <div id="app" class="container view-container">
    <ChatBot/>
  </div>
</template>

<script>
import ChatBot from './components/ChatBot';

export default {
  name: 'App',
  components: {
    ChatBot
  },
  data() {
    return {
      msg: 'Hello'
    }
  }
}
</script>

<style>
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
  body {
    font-family: Arial, Helvetica, sans-serif;
    line-height: 1.4;
  }
  .view-container {
    width: 50%;
  }
</style>
