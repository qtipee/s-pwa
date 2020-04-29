<template>
    <div>
        <!--FileExplorer /-->
        <CRUDTable />
    </div>
</template>

<script>
import FileExplorer from '~/components/FileExplorer.vue'
import CRUDTable from '~/components/CRUDTable.vue'

export default {

    // TEMPORARY: allows access to the index page without being logged in
    auth: false,

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
