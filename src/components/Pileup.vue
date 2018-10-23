<template>
  <v-container>
    <v-card>
    <div id='igv-content'></div>
    <v-layout justify-center>
      <v-btn fab small @click='zoomOut'>
        <v-icon>zoom_out</v-icon>
      </v-btn>
      <v-btn fab small @click='zoomIn'>
        <v-icon>zoom_in</v-icon>
      </v-btn>
    </v-layout>
    </v-card>
  </v-container>
</template>

<script>

import igv from 'igv'

const PileupButton = {
  render(h) {
    return h('button', {
        on: {
          click: () => {
            this.$emit('click');
          }
        },
      },
      this.$slots.default
    );
  },
};

export default {
  name: 'pileup',
  props: {
    referenceURL: String,
    alignmentURL: String,
    locus: String,
    visible: Boolean,
  },
  components: {
    PileupButton,
  },
  data () {
    return {
      browser: null,
    }
  },
  mounted: function () {
    this.init();
  },
  methods: {
    init: function() {
      const igvDiv = this.$el.querySelector('#igv-content');
   
      var options = {
        showControls: false,
        showIdeogram: false,
        showTrackLabels: false,
        //showCenterGuide: true,
        minimumBases: 20,
        reference: {
         fastaURL: this.referenceURL,
        },
        locus: this.locus,
        tracks: [
          {
            type: 'alignment',
            format: 'bam',
            url: this.alignmentURL,
            //name: 'HG02450'
          }
        ],
        //showCursorTrackingGuide: true,
        //showSequence: false,
        //showNavigation: false,
        //showRuler: false,
        //flanking: 1000,
        //locus: 'chr8:128748750-128749000',
      }

      igv.createBrowser(igvDiv, options).then((browser) => {
        this.browser = browser;
        console.log(browser);
      })
    },
    zoomOut: function() {
      this.browser.zoomOut();
    },
    zoomIn: function() {
      this.browser.zoomIn();
    },
  },
  watch: {
    visible: function() {
      igv.removeBrowser(this.browser);
      this.init();
      //igv.visibilityChange();
    },
    locus: function() {
      console.log("locus change: " + this.locus);
    }
  }
}
</script>

<style scoped>
</style>
