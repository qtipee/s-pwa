<template>
    <div class="file-explorer">
        <h1>TITLE: {{ test }}</h1>
        <v-row justify="space-between">

            <v-col cols="12">
                <v-text-field
                    v-model="search"
                    label="Search..."
                    solo-inverted
                    hide-details
                    clearable
                ></v-text-field>
            </v-col>

            <v-col cols="6">
                <v-treeview
                    v-model="tree"
                    :items="items"
                    item-key="name"
                    :search="search"
                    :filter="filter"
                    :open="open"
                    open-on-click
                    activatable
                    :active.sync="active"
                    return-object
                    selectable
                    dense
                    transition
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
            </v-col>

            <v-divider vertical></v-divider>

            <v-col class="text-center">
                <div
                    v-if="!selected"
                    class="title white--text text--lighten-1 font-weight-light"
                    style="align-self: center;"
                >
                    Select a file
                </div>
                <div
                    class="file-info"
                    v-else
                >
                    <v-text-field
                        :value="selectedValue"
                    ></v-text-field>
                    <div class="file-fab">
                        <v-btn class="mx-1" fab small outlined>
                            <v-icon>mdi-pencil-outline</v-icon>
                        </v-btn>
                        <v-btn class="mx-1" fab small outlined>
                            <v-icon>mdi-trash-can-outline</v-icon>
                        </v-btn>
                    </div>
                </div>
            </v-col>

        </v-row>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data () {
        return {
            open: [],
            active: [],
            search: null,

            test: '',

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
            //FIXME: temporary
            items: [
                /*
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
                */
            ],
        }
    },

    // Basic components do not have an asyncData method ; use mounted or
    // asyncData from the page component and pass the data as props
    mounted () {
        /*
        return axios.get(`http://127.0.0.1:8000/test`)
            .then((res) => {
                return { test: res.data.title}
            })
            .catch((err) => {
                console.log('[ERROR]', err)
            })
            */
    },

    computed: {
        filter () {
            return (item, search, textKey) => item[textKey].indexOf(search) > -1
        },

        selected () {
            if (!this.active.length) return undefined

            return this.active[0]
        },

        selectedValue () {
            return this.selected.name
        },
    },
}
</script>
