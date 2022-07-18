<template>
  <v-container fluid>
    <v-text-field
      label="Title"
      style="direction: rtl"
      class="d-print-none"
      v-model="title"/>
    <v-text-field
      label="Title (reading aid)"
      class="d-print-none"
      v-model="title_aid"/>
    <v-textarea
      name="input-7-1"
      filled
      style="direction: rtl"
      class="d-print-none"
      label="Arabic Text"
      v-model="arabic"
      auto-grow
    ></v-textarea>
    <v-card class="d-print-none">
     <v-card-title>Generated Transcription</v-card-title>
      <v-card-text>
        <span v-html="script"/>
      </v-card-text>
    </v-card>
    <v-textarea
      name="input-7-1"
      filled
      class="d-print-none"
      label="Reading Aid"
      v-model="aid"
      auto-grow
    ></v-textarea>
    <v-card flat>
      <v-card-text>
        <v-row>
          <v-btn dark small color="black" @click="print" class="d-print-none">
            <v-icon>mdi-printer</v-icon>
          </v-btn>
          <v-spacer/>
          <p class="title text-h4" style="direction: rtl"><arabic-line :arabic="title" :aid="title_aid"/></p>
        </v-row>
        <v-row>
          <v-col>
            <p style="direction: rtl">
              <arabic-line v-for="(arabic, i) of arabics" :key="i" :arabic="arabic" :aid="aids[i]" />
            </p>
          </v-col>
        </v-row>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script lang='ts'>
import { defineComponent } from 'vue'
import ArabicLine from "./ArabicLine.vue";
import arabscript from "arabscript";

export default defineComponent({
  data() {
    return {
      title: "اليَوْمُ الأوَّلُ: النُّور",
      title_aid: "a-l-ya-w-mu (a)-l-'a-wwa-lu: 'a-(l)-nn-ū-r",
      arabic: `
فِي البَدءِ خَلَقَ اللهُ السَّمَاوَاتِ وَالأرْضَ.
كَانَتِ الأرْضُ قَاحِلَةً وَفَارِغَةً.
وَكَانَ الظَّلَامُ يَلُفُّ المُحِيطَ، وَرُوحُ اللهِ تُحَوِّمُ فَوْقَ المِيَاهِ.
فِي ذَلِكَ الوَقْتِ، قَالَ اللهُ: «لِيَكُنْ نُورٌ.»
 فَصَارَ نُورٌ
      `,
      aid: `
f:ī (a):l-ba:d:'i ẖa:la:qa (a):l:la:hu as-sa:mā:wā:ti wa-(a):l'a:r:ḍa. 
k:ā:na:ti (a):l'a:r:ḍu q:ā:ḥi:la:tan wa-f:ā:ri:ġa:tan. 
wa-k:ā:na (a):(l)-ẓẓa:l:ā:mu ya:la:ffu (a):l-mu:ḥi:ṭa, wa-r:ū:ḥu (a):l:lā:hi tu-ḥa:wwa:mu fa:w:qa (a):l-m:ī:ā:ḥi.
f:ī ḏā:li:ka (a):l-wa:q:ti q:ā:la (a):l:lā:hu: "li-ya-ku:n n:ū:ru."
fa-ṣ:ā:ra n:ū:ru.
      `,
    };
  },

  components: { ArabicLine },

  computed: {
    arabics() {
      return this.arabic.trim().split('\n');
    },
    aids() {
      return this.aid.trim().split('\n');
    },
    script() {
      return this.arabics.map(arabscript).join('<br/>');
    }
  },

  methods: {
    print() {
      window.print();
    },
  }
})
</script>
