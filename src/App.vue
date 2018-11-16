<template>
  <div id="vue-pileup-test-app">
    <pileup
      referenceURL="https://s3.amazonaws.com/igv.broadinstitute.org/genomes/seq/hg19/hg19.fasta"
      locus="chr21:45712947-45713007"
      :tracks='tracks'
    />
  </div>
</template>

<script>
import Pileup from './components/Pileup.vue'

export default {
  name: 'vue-pileup-test-app',
  components: {
    Pileup
  },
  computed: {
    tracks: function() {
      return [
        {
          name: 'Proband',
          alignmentURL: "https://s3.amazonaws.com/iobio/samples/bam/NA12878.exome.bam",
          variantURL: "https://s3.amazonaws.com/iobio/samples/vcf/platinum-exome.vcf.gz",
        },
        {
          name: 'Mother',
          alignmentURL: "https://s3.amazonaws.com/iobio/samples/bam/NA12878.exome.bam",
          variantURL: "https://s3.amazonaws.com/iobio/samples/vcf/platinum-exome.vcf.gz",
        },
      ]
    }
  }
}
</script>

<style>
#vue-pileup-test-app {
  width: 800px;
}
</style>
