<template>
  <span class="word" :class="{firefox, chrome: !firefox}">
    <ruby v-if="firefox">
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
    <ruby v-else>
      <span class="arabic">
        <span v-for="([char, odd], i) of arabic_lights" :key="i" :class="{odd}">
          {{ char }}
        </span>
      </span>
      <rt>
        <span class="transcription">
          <span>
            {{ transcription.replace(/[,.:"?!]/g, '') }}
          </span>
        </span>
        <span class="aid">
          <span v-for="(char, i) of aids" :key="i" :class="{odd: i % 2}">
            {{ char }}
          </span>
        </span>
      </rt>
    </ruby>
  </span>
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
    },
    firefox() {
      return /firefox/i.test(navigator.userAgent);
    }
  },
})
</script>
<style lang="scss" scoped>
.word {
  position: relative;
  margin-left: 1ex;
  margin-right: 1ex;

  .odd {
    color: cornflowerblue;
  }

  .aid {
    direction: ltr;

    span {
      padding: .3ex;
      display: inline-block;
    }
  }

  .transcription {
    span {
      display: inline-block;
    }

    direction: ltr;
  }
}

.word.firefox {
  .aid {
    position: relative;
    top: .8em;
  }
  .transcription {
    position: relative;
    top: -.8em;
  }
}

.word.chrome {
  .aid {
    text-align: center;

    span {
      padding-bottom: .6em;
    }
  }

  .transcription {
    position: absolute;
    top: 3.4em;
    left: .2em;
    white-space: nowrap;
  }
}
</style>
