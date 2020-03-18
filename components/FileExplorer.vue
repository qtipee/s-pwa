<template>
    <div class="file-explorer">
        <v-text-field
            v-model="search"
            label="Search..."
            solo-inverted
            hide-details
            clearable
        ></v-text-field>
        <v-treeview
            v-model="tree"
            :search="search"
            :filter="filter"
            :open="open"
            :items="items"
            item-key="name"
            open-on-click
            dense
        >
            <template v-slot:prepend="{ item, open }">
                <v-icon v-if="!item.file">
                    {{ open ? 'mdi-folder-open' : 'mdi-folder' }}
                </v-icon>
                <v-icon v-else>
                    {{ files[item.file] }}
                </v-icon>
            </template>
        </v-treeview>
    </div>
</template>

<script>
export default {
    data () {
        return {
            open: ['public'],
            search: null,
            files: {
                html: 'mdi-language-html5',
                js: 'mdi-nodejs',
                json: 'mdi-json',
                md: 'mdi-markdown',
                pdf: 'mdi-file-pdf',
                png: 'mdi-file-image',
                txt: 'mdi-file-document-outline',
                xls: 'mdi-file-excel',
            },
            tree: [],
            items: [
                {
                    name: '.git',
                },
                {
                    name: 'node_modules',
                },
                {
                    name: 'public',
                    children: [
                        {
                            name: 'static',
                            children: [{
                                name: 'logo.png',
                                file: 'png',
                            }],
                        },
                        {
                            name: 'favicon.ico',
                            file: 'png',
                        },
                        {
                            name: 'index.html',
                            file: 'html',
                        },
                    ],
                },
                {
                    name: '.gitignore',
                    file: 'txt',
                },
                {
                    name: 'babel.config.js',
                    file: 'js',
                },
                {
                    name: 'package.json',
                    file: 'json',
                },
                {
                    name: 'README.md',
                    file: 'md',
                },
                {
                    name: 'vue.config.js',
                    file: 'js',
                },
                {
                    name: 'yarn.lock',
                    file: 'txt',
                },
            ],
        }
    },
    computed: {
        filter () {
            return (item, search, textKey) => item[textKey].indexOf(search) > -1
        }
    },
}
</script>
