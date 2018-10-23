<template>
  <div id="app">
    <pileup
        referenceURL="https://s3.amazonaws.com/igv.broadinstitute.org/genomes/seq/hg19/hg19.fasta"
        alignmentURL="https://s3.amazonaws.com/iobio/samples/bam/NA12878.exome.bam"
        locus="chr21:45712947-45713007"
    />
  </div>
</template>

<script>
import Pileup from './components/Pileup.vue'

export default {
  name: 'app',
  components: {
    Pileup
  }
}
</script>

<style>
#app {
  height: 200px;
}
</style>
