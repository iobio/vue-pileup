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


export default {
  name: 'pileup',
  props: {
    referenceURL: String,
    alignmentURL: String,
    locus: String,
    visible: {
      type: Boolean,
      default: true,
    }
  },
  data () {
    return {
      browser: null,
    }
  },
  mounted: function () {
    if (this.visible) {
      this.init();
    }
  },
  methods: {
    init: function() {
      const igvDiv = this.$el.querySelector('#igv-content');
   
      var options = {
        showControls: false,
        showIdeogram: false,
        showTrackLabels: false,
        showCenterGuide: true,
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
      if (!this.browser) {
        this.init();
      }
      else if (!this.visible) {
        igv.removeBrowser(this.browser);
        this.browser = null;
      }
      //igv.visibilityChange();
    },
  }
}
</script>

<style scoped>
</style>
