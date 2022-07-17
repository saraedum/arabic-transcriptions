<template>
  <v-app>
    <v-main>
      <Editor/>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import Editor from './components/Editor.vue'

export default defineComponent({
  name: 'App',

  components: {
    Editor,
  },

  data () {
    return {}
  },
})
</script>
