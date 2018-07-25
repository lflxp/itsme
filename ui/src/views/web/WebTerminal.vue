<template>
    <div class="console" id="terminal"></div>
</template>
<script>
import { Terminal } from 'xterm'
import * as fit from 'xterm/lib/addons/fit/fit'
import * as attach from 'xterm/lib/addons/attach/attach'
import * as fullscreen from 'xterm/lib/addons/fullscreen/fullscreen'
Terminal.applyAddon(fit)
Terminal.applyAddon(attach)
Terminal.applyAddon(fullscreen)

export default {
  name: 'WebTerminal',
  props: {
    terminal: {
      type: Object,
      default: {}
    }
  },
  data() {
    return {
      term: null,
      terminalSocket: null
    }
  },
  methods: {
    ab2str(buf) {
      return String.fromCharCode.apply(null, new Uint8Array(buf))
    },
    runRealTerminal(evt) {
      console.log(evt)
      // this.term.on('data', function(evt) {
      //   console.log(1, evt)
      //   this.terminalSocket.send(new TextEncoder().encode('\x00' + evt))
      //   console.log(2)
      // })

      // this.term.on('resize', function(evt) {
      //   console.log('1111', evt)
      //   this.terminalSocket.send(new TextEncoder().encode('\x01' + JSON.stringify({ cols: evt.cols, rows: evt.rows })))
      // })

      // this.term.on('title', function(title) {
      //   document.title = title
      // })

      // this.term.fit()
      // this.term.toggleFullScreen(true) // Now the terminal should be back to normal
      // // term.toggleFullScreen(false);  // Now the terminal should be back to normal
      // this.terminalSocket.onmessage = function(evt) {
      //   if (evt.data instanceof ArrayBuffer) {
      //     this.term.write(this.ab2str(evt.data))
      //   } else {
      //     alert(evt.data)
      //   }
      // }
      console.log('webSocket is finished')
    },
    errorRealTerminal(evt) {
      // if (typeof console.log === 'function') {
      //   console.log(evt)
      // }
      console.log('error')
    },
    closeRealTerminal() {
      // this.term.write('Session terminated')
      // this.term.destroy()
      console.log('close')
    }
  },
  mounted() {
    console.log('pid : ' + this.terminal.pid + ' is on ready')
    // Terminal.applyAddon(Terminal)
    const terminalContainer = document.getElementById('terminal')
    this.term = new Terminal()
    this.term.open(terminalContainer)
    // open websocket
    // var websocket = new WebSocket("ws://" + window.location.hostname + ":" + window.location.port + "/term");
    this.terminalSocket = new WebSocket('ws://127.0.0.1:3000/term')
    this.terminalSocket.binaryType = 'arraybuffer'
    this.terminalSocket.onopen = this.runRealTerminal
    this.terminalSocket.onclose = this.closeRealTerminal
    this.terminalSocket.onerror = this.errorRealTerminal
    this.term.attach(this.terminalSocket)
    this.term._initialized = true
    console.log('mounted is going on')
  },
  beforeDestroy() {
    this.terminalSocket.close()
    this.term.destroy()
  }
}
</script>