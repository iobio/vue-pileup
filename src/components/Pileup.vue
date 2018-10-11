<template>
  <div class='pileup'>
    <button @click='zoomOut'>-</button>
    <button @click='zoomIn'>+</button>
    <div id='igv-content'></div>
  </div>
</template>

<script>

import igv from 'igv'

export default {
  name: 'pileup',
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
      reference: {
       id: "hg38",
       name: "Human (GRCh38/hg38)",
       fastaURL: "https://s3.amazonaws.com/igv.broadinstitute.org/genomes/seq/hg38/hg38.fa",
       indexURL: "https://s3.amazonaws.com/igv.broadinstitute.org/genomes/seq/hg38/hg38.fa.fai",
      },
      //locus: 'chr8:128,747,267-128,748,546',
      locus: 'chr8:128748750-128749000',
      flanking: 1000,
      tracks: [
        {
          type: 'alignment',
          format: 'bam',
          url: 'https://s3.amazonaws.com/1000genomes/phase3/data/HG00096/exome_alignment/HG00096.mapped.ILLUMINA.bwa.GBR.exome.20120522.bam',
          name: 'HG02450'
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
#igv-content {
  max-width: 800px;
  max-height: 400px;
  pointer-events: none;
}
</style>
