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
      title: "الفاتحة",
      title_aid: "a:l-f:ā:ti:ḥ:a",
      arabic: `
بِسۡمِ ٱللَّهِ ٱلرَّحۡمَٰنِ ٱلرَّحِيمِ
ٱلۡحَمۡدُ لِلَّهِ رَبِّ ٱلۡعَٰلَمِينَ
ٱلرَّحۡمَٰنِ ٱلرَّحِيمِ
مَٰلِكِ يَوۡمِ ٱلدِّينِ
إِيَّاكَ نَعۡبُدُ وَإِيَّاكَ نَسۡتَعِينُ
ٱهۡدِنَا ٱلصِّرَٰطَ ٱلۡمُسۡتَقِيمَ
صِرَٰطَ ٱلَّذِينَ أَنۡعَمۡتَ عَلَيۡهِمۡ غَيۡرِ ٱلۡمَغۡضُوبِ عَلَيۡهِمۡ وَلَا ٱلضَّآلِّينَ
      `,
      aid: `
bi:s:mi (a):l:lā:hi (a):(l)-ra:ḥ:mā:ni (a):(l)-rra:ḥ:ī:m
'a:l-ḥa:m:du li-lā:hi ra:bbu (a):l-ʿā:la:m:ī:n
'a:(l)-rra:ḥ:mā:ni (a):(l)-rra:ḥ:ī:m
mā:li:ki ya:w:mi (a):(l)-dd:ī:n
'i:yy:ā:ka na-ʿa:bu:du wa-'i:yy:ā:ka na-s:ta:ʿ:ī:n
'i:h:di-n:ā (a):(l)-ṣṣi:rā:ṭa (a):l-mu:s:ta:q:ī:m
ṣṣi:rā:ṭa (a):lla:ḍ:ī:n 'a:n:ʿa:m-ta ʿa:la:y:hi:m ġa:y:ri (a):l:ma-ġ:ḍ:ū:bi  ʿa:la:y-hi:m wa-l:ā (a):(l)-ḍḍ:ā:ll:ī:n
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
