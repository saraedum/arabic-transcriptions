<template>
  <p class="line">
    <arabic-word v-for="(arabic, i) of arabics" :key="i" :arabic="arabic" :aid="aids[i]" />
  </p>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
import ArabicWord from "./ArabicWord.vue";

export default defineComponent({
  props: {
    arabic: {
      type: String,
      required: true,
    },
    transcription: String,
    aid: String,
  },

  components: { ArabicWord },

  computed: {
    arabics() {
      return this.arabic.trim().split(/\s+/).filter(x => x);
    },
    transcriptions() {
      return (this.transcription || '').trim().split(/\s+/);
    },
    aids() {
      return (this.aid || '').trim().split(/\s+/);
    },
  },
})
</script>
<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css?family=Noto+Sans');
@import url('https://fonts.googleapis.com/css?family=Noto+Sans+Arabic');

.line {
  font-size: 18pt;
  line-height: 4;
  font-family: 'Noto Sans Arabic', 'Noto Sans', sans-serif;
}

.title .line {
  font-size: 22pt;
  font-weight: bold;
}
</style>
