<template>
    <!--FileExplorer /-->
    <!--CRUDTable /-->

    <div>
        
    </div>

</template>

<script>
import FileExplorer from '~/components/FileExplorer.vue'
import CRUDTable from '~/components/CRUDTable.vue'

export default {
  components: {
      FileExplorer,
      CRUDTable,
  },

  data () {
    return {
        tests: []
    }
  },

  async asyncData ({ $axios, params }) {
      /*
      try {
          let t = await $axios.$get('/tests/')
          console.log(t)
          return { t }
      } catch (e) {
          return { tests: [] }
      }
      */
  }
}
</script>
