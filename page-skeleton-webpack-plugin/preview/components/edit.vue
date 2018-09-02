<template>
  <div class="edit-panel">
    <div ref="editor"></div>
  </div>
</template>

<script>
  import codeMirror from '../codeMirror'
  import bus from '../bus'
  export default {
    data () {
      return {
        editor: null
      }
    },
    props: {
      html: {
        type: String,
        required: true
      },
      currentRoute: {
        type: String,
        required: true
      }
    },
    created() {
      this.$nextTick(() => {
        const { html } = this
        const container = this.$refs.editor
        const codeMirrorConfig = {
          value: html,
          lineNumbers: true,
          autofocus: true,
          lineWrapping: true,
          styleActiveLine: true
        }
        const editor = this.editor = codeMirror(container, codeMirrorConfig)
        editor.on('change', (cm, change) => {
          const html = cm.getValue()
          console.log(html,'77777777');
          // debugger
          this.$store.dispatch('SAVE_CODE', { route: this.currentRoute, html })
        })
        bus.$on('set-code', this.setCode)
      })
    },
    beforeDestroy() {
      bus.$off('set-code', this.setCode)
    },
    methods: {
      setCode (routesData) {
        const { editor } = this
        console.log(routesData,'11111111');
        let html = routesData.html
        console.log(this.currentRoute,'222222');
        if (editor) editor.setValue(html)

        // if (editor) editor.setValue(routesData.html)
        // if (editor) editor.setValue(routesData[this.currentRoute].html)

      }
    }
  }
</script>

<style scoped>
</style>