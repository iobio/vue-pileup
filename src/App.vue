<template>
  <div id="app">
    <pileup
        referenceURL="https://s3.amazonaws.com/igv.broadinstitute.org/genomes/seq/hg38/hg38.fa"
        alignmentURL="https://s3.amazonaws.com/1000genomes/phase3/data/HG00096/exome_alignment/HG00096.mapped.ILLUMINA.bwa.GBR.exome.20120522.bam"
        locus="chr8:128748750-128749000"
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
</style>
