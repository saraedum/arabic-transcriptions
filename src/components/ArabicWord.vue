<template>
  <span class="word">
    <ruby>
      <rbc>
        <span class="arabic">
          <span v-for="([char, odd], i) of arabic_lights" :key="i" :class="{odd}">
            {{ char }}
          </span>
        </span>
      </rbc>
      <rtc class="aid">
        <rt>
          <span v-for="(char, i) of aids" :key="i" :class="{odd: i % 2}">
            {{ char }}
          </span>
        </rt>
      </rtc>
      <rtc class="transcription">
        <rt>
          <span>
            {{ transcription.replace(/[,.:"?!]/g, '') }}
          </span>
        </rt>
      </rtc>
    </ruby>
  </span>&nbsp;
</template>
<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    arabic: {
      type: String,
      required: true,
    },
    aid: String,
  },
  computed: {
    arabics() {
      return this.arabic.trim().split(/(?=[^ ؘ َ  ُ  ِ  ّ  ْ  ٓ  ٔ  ٕ])/)
    },
    arabic_lights() {
      let light = false;
      const lights = [];
      for (const char of this.arabics) {
        if (char.match(/[\u0600-\u06FF]/)) {
          lights.push([char, light]);
          light = !light;
        } else {
          lights.push([char, false]);
        }
      }
      return lights;
    },
    transcription() {
      return (this.aid || '').replace(/:/g, '')
    },
    aids() {
      const aid = (this.aid || '').replace(/[,."?!]/g,'').trim()
      const aids = aid.split(/:|(?=-)|(?<=-)/).filter((aid) => aid);
      const chars = [] as Array<string>;

      for (let char of aids) {
        if (char == '-') {
          if(chars.length % 2)
            chars[chars.length - 1] = `- ${chars[chars.length - 1]}`;
          else
            chars.push(char);
        } else {
          if (chars[chars.length - 1] == "-") {
            char = `${char} -`;
            chars.pop();
          }
          chars.push(char);
        }
      }
      return chars;
    }
  },
})
</script>
<style lang="scss" scoped>
.word {
  .odd {
    color: cornflowerblue;
  }
  rtc.aid {
    position: relative;
    direction: ltr;
    top: .8em;

    span {
      padding: .3ex;
      display: inline-block;
    }
  }
  rtc.transcription {
    position: relative;
    top: -.8em;

    span {
      display: inline-block;
      direction: ltr;
    }
  }
}
</style>
