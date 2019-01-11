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
    locus: String,
    visible: {
      type: Boolean,
      default: true,
    },
    tracks: Array,
    showLabels: {
      type: Boolean,
      default: false,
    },
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
   
      const options = {
        showControls: false,
        showIdeogram: false,
        showTrackLabels: this.showLabels,
        showCenterGuide: true,
        minimumBases: 20,
        reference: {
         fastaURL: this.referenceURL,
        },
        locus: this.locus,
        tracks: [],
        //showCursorTrackingGuide: true,
        //showSequence: false,
        //showNavigation: false,
        //showRuler: false,
        //flanking: 1000,
        //locus: 'chr8:128748750-128749000',
      }

      for (const track of this.tracks) {

        if (track.variantURL) {
          // insert variant track as first track
          options.tracks.push({
            name: track.name + " Variants", 
            type: "variant",
            format: "vcf",
            url: track.variantURL,
            squishedCallHeight: 1,
            expandedCallHeight: 4,
            displayMode: "squished",
            visibilityWindow: 1000
          });
        }
        
        options.tracks.push({
          name: track.name + " Alignment", 
          type: 'alignment',
          format: 'bam',
          url: track.alignmentURL,
          indexURL: track.alignmentIndexURL,
        });
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

<style>

.igv-right-hand-gutter {
  right: initial;
  left: -10px;
}

.igv-content-header {
  display: none;
}

</style>
