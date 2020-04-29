<template>
    <div>
        <!--FileExplorer /-->
        <!--CRUDTable /-->
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
    }
}
</script>
