<template>
  <div class="container">
    <web-terminal :terminal="terminal"></web-terminal>
  </div>
</template>

<script>
import WebTerminal from './WebTerminal'
export default {
  name: 'WebSSH',
  data() {
    return {
      terminal: {
        pid: 1,
        name: 'terminal',
        cols: 150,
        rows: 150
      }
    }
  },
  components: {
    WebTerminal
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>