<template>
  <div class='pileup'>
    <div id='igv-content'></div>
    <div id='igv-zoom-buttons'>
      <button @click='zoomOut'>-</button>
      <button @click='zoomIn'>+</button>
    </div>
  </div>
</template>

<script>

import igv from 'igv'

export default {
  name: 'pileup',
  props: {
    referenceURL: String,
    alignmentURL: String,
    locus: String,
  },
  data () {
    return {
      browser: null,
    }
  },
  mounted: function () {
    const igvDiv = this.$el.querySelector('#igv-content');

    var options = {
      //showNavigation: false,
      //showRuler: false,
      showControls: false,
      showIdeogram: false,
      showTrackLabels: false,
      //showCursorTrackingGuide: true,
      showCenterGuide: true,
      //showSequence: false,
      minimumBases: 20,
      reference: {
       //id: "hg38",
       //name: "Human (GRCh38/hg38)",
       fastaURL: this.referenceURL,
      },
      //locus: 'chr8:128748750-128749000',
      locus: this.locus,
      //flanking: 1000,
      tracks: [
        {
          type: 'alignment',
          format: 'bam',
          url: this.alignmentURL,
          //name: 'HG02450'
        }
      ]
    }

    igv.createBrowser(igvDiv, options).then((browser) => {
      this.browser = browser;
    })
  },
  methods: {
    zoomOut: function() {
      this.browser.zoomOut();
    },
    zoomIn: function() {
      this.browser.zoomIn();
    },
  },
}
</script>

<style scoped>
.pileup {
  max-width: 640px;
}

#igv-content {
  max-height: 480px;
  /*pointer-events: none;*/
}

#igv-zoom-buttons {
  display: flex;
  flex-direction: row;
  justify-content: center;
}
</style>
