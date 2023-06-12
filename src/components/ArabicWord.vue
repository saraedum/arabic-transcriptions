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

const special = new RegExp(`(?=[^${[
  '\u064B', // Fathatan
  '\u064C', // Dammatan
  '\u064D', // Kasratan
  '\u064E', // Fatha
  '\u064F', // Damma
  '\u0650', // Kasra
  '\u0651', // Shadda
  '\u0652', // Sukun
  '\u0653', // Maddah Above
  '\u0654', // Hamza Above
  '\u0655', // Hamza Below
  '\u0656', // Subscript Alef
  '\u0657', // Inverted Damma
  '\u0658', // Mark Noon Ghunna
  '\u0659', // Zwarakay
  '\u065A', // Vowel Sign Small V Above
  '\u065B', // Vowel Sign Inverted Small V Above
  '\u065C', // Vowel Sign Dot Below
  '\u065D', // Reversed Damma
  '\u065E', // Fatha With Two Dots
  '\u065F', // Wavy Hamza Below
  '\u0670', // Letter Superscript Alef
  '\u06e1', // Small High Dotless Head Of Khah
  '\u06e4', // Small High Madda
  '\u06e5', // Small Waw
  '\u06e6', // Small Yeh
  '\u0615', // Small High Tah
  '\u0616', // Small High Ligature Alef With Lam With Yeh
  '\u0617', // Smal High Zain
  '\u0618', // Small Fatha
  '\u0619', // Small Damma
  '\u061A', // Small Kasra
  '\u06D6', // Small High Ligature Sad With Lam With Alef Maksura
  '\u06D6', // Small High Ligature Sad With Lam With Alef Maksura
  '\u06D7', // Small High Ligature Qaf With Lam With Alef Maksura
  '\u06D8', // Small High Meem Initial Form
  '\u06D9', // Small High Lam Alef
  '\u06DA', // Small High Jeem
  '\u06DB', // Small High Three Dots
  '\u06DC', // Small High Seen
  '\u06DD', // End of Ayah
  '\u06DE', // Star of Rub El Hizb
  '\u06DF', // Small High Rounded Zero
  '\u06E0', // Small High Upright Rectangular Zero
  '\u06E1', // Small High Dotless Head Of Khah
  '\u06E2', // Small High Meem Isolated Form
  '\u06E3', // Small Low Seen
  '\u06E4', // Small High Madda
  '\u06E5', // Small Waw
  '\u06E6', // Small Yeh
  '\u06E7', // Small High Yeh
  '\u06E8', // Small High Noon
  '\u08C9-\u08FF', // Small Characters in Arabic Extended A
].join('')}])`);

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
      return this.arabic.trim().split(special)
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

      function upper(aid: string) {
        if (aid.length >= 2 && aid[0] === aid[1])
          return `${aid[0].toUpperCase()}${aid[1].toUpperCase()}${aid.substring(2)}`;
        return `${aid[0].toUpperCase()}${aid.substring(1)}`;
      }

      const aids = aid.split(/:|(?=-)|(?<=-)/).filter((aid) => aid).map(upper);
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
    direction: ltr;
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
