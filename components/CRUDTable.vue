<template>
    <v-data-table
    :headers="headers"
    :items="items"
    sort-by="calories"
    >
        <template v-slot:top>
            <v-toolbar>
                <v-toolbar-title>{{ tableName }}</v-toolbar-title>
                <v-divider
                    inset
                    vertical
                ></v-divider>
                <v-spacer></v-spacer>
                <v-dialog v-model="dialog">
                    <template v-slot:activator="{ on }">
                        <v-btn color="primary" v-on="on">New Item</v-btn>
                    </template>
                    <v-card>
                        <v-card-title>
                            <span class="headline">{{ formTitle }}</span>
                        </v-card-title>
                        <v-card-text>
                            <v-container>
                                <v-row>
                                    <v-col cols=12 v-for="(field, i) in fields" :key="i">
                                        <v-text-field
                                            v-if="field.component === 'v-text-field'"
                                            :v-model="editedItem[field.name]"
                                            :label="field.label"
                                            :type="field.type"
                                        ></v-text-field>
                                        <v-textarea
                                            v-if="field.component === 'v-textarea'"
                                            :v-model="editedItem[field.name]"
                                            :label="field.label"
                                        ></v-textarea>
                                        <v-switch
                                            v-if="field.component === 'v-switch'"
                                            :v-model="editedItem[field.name]"
                                            :label="field.label"
                                        ></v-switch>
                                        <v-select
                                            v-if="field.component === 'v-select'"
                                            :v-model="editedItem[field.name]"
                                            :label="field.label"
                                            :items="field.items"
                                        ></v-select>
                                        <v-file-input
                                            v-if="field.component === 'v-file-input'"
                                            :v-model="editedItem[field.name]"
                                            :label="field.label"
                                            :prepend-icon="field.icon"
                                        ></v-file-input>
                                        <v-menu
                                            v-if="field.component === 'v-date-picker'"
                                            :nudge-right="40"
                                            transition="scale-transition"
                                            offset-y
                                        >
                                            <template v-slot:activator="{ on }">
                                                <v-text-field
                                                    :v-model="editedItem[field.name]"
                                                    :label="field.label"
                                                    prepend-icon="mdi-calendar"
                                                    readonly
                                                    v-on="on"
                                                ></v-text-field>
                                            </template>
                                            <v-date-picker
                                                :v-model="editedItem[field.name]"
                                                no-title
                                            ></v-date-picker>
                                        </v-menu>
                                    </v-col>
                                </v-row>
                            </v-container>
                        </v-card-text>
                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
                            <v-btn color="blue darken-1" text @click="save">Save</v-btn>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
            </v-toolbar>
        </template>
        <template v-slot:item.actions="{ item }">
            <v-icon class="mr-1" @click="editItem(item)">
                mdi-pencil
            </v-icon>
            <v-icon class="mr-1" @click="deleteItem(item)">
                mdi-delete
            </v-icon>
        </template>
        <template v-slot:no-data>
            <v-btn color="primary" @click="initialize">Reset</v-btn>
        </template>
    </v-data-table>
</template>

<script>
export default {
    data: () => ({
        tableName: 'CRUD Test',
        dialog: false,        
        headers: [],
        fields: [],
        items: [],
        editedIndex: -1,
        editedItem: {
            name: '',
            calories: 0,
            fat: 0,
            carbs: 0,
            protein: 0,
        },
        defaultItem: {
            name: '',
            calories: 0,
            fat: 0,
            carbs: 0,
            protein: 0,
        },
    }),

    computed: {
        formTitle () {
            return this.editedIndex === -1 ? 'New Item' : 'Edit Item'
        },
    },

    watch: {
        dialog (val) {
            val || this.close()
        },
    },

    created () {
        this.initialize()
    },

    methods: {
        initialize () {

            this.fields = [
                {
                    name: 'id',
                    component: 'v-text-field',
                    label: 'ID',
                    type: 'number'
                },
                {
                    name: 'title',
                    component: 'v-text-field',
                    label: 'Title',
                    type: 'text'
                },
                {
                    name: 'text',
                    component: 'v-textarea',
                    label: 'Text'
                },
                {
                    name: 'boolean',
                    component: 'v-switch',
                    label: 'Boolean'
                },
                {
                    name: 'date',
                    component: 'v-date-picker',
                    label: 'Date'
                },
                {
                    name: 'select',
                    component: 'v-select',
                    label: 'Select',
                    items: ['Item 1', 'Item 2', 'Item 3', 'Item 4']
                },
                {
                    name: 'image',
                    component: 'v-file-input',
                    label: 'Image',
                    icon: 'mdi-image'
                },
                {
                    name: 'file',
                    component: 'v-file-input',
                    label: 'File',
                    icon: 'mdi-paperclip'
                }
            ]

            this.fields.forEach(field => {
                this.headers.push({ text: field.label, value: field.name })
            })
            this.headers.push({ text: 'Actions', value: 'actions', sortable: false })

            this.items = [
            ]
        },

        editItem (item) {
            this.editedIndex = this.items.indexOf(item)
            this.editedItem = Object.assign({}, item)
            this.dialog = true
        },

        deleteItem (item) {
            const index = this.items.indexOf(item)
            confirm('Are you sure you want to delete this item?') && this.items.splice(index, 1)
        },

        close () {
            this.dialog = false
            setTimeout(() => {
                this.editedItem = Object.assign({}, this.defaultItem)
                this.editedIndex = -1
            }, 300)
        },

        save () {
            if (this.editedIndex > -1) {
                Object.assign(this.items[this.editedIndex], this.editedItem)
            } else {
                this.items.push(this.editedItem)
            }
            this.close()
        }
    }
}
</script>
