<template>
    <v-data-table
      :headers="headers"
      :items="items"
      sort-by="calories"
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar
          flat
        >
          <v-toolbar-title>{{ cardTitle }}</v-toolbar-title>
          <v-divider
            class="mx-4"
            inset
            vertical
          ></v-divider>
          <v-spacer></v-spacer>
          <v-dialog
            v-model="dialog"
            max-width="90vw"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                color="primary"
                dark
                class="mb-2"
                v-bind="attrs"
                v-on="on"
                fab
                small
              >
                <v-icon>
                  mdi-plus
                </v-icon>
              </v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="text-h5">{{ formTitle }}</span>
              </v-card-title>
  
              <v-card-text>
                <v-container>
                  <v-row dense justify="center">
                    <v-col
                      v-for="(inp,inpIndex) of editedItem"
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-if="!inpIndex.includes('date')"
                        v-model="editedItem[inpIndex]"
                        :label="inpIndex.toUpperCase()"
                        outlined
                        dense
                        hide-details
                      ></v-text-field>
  
                      <select-date v-else @date="date=>editedItem[inpIndex]=date"
                                   :label="inpIndex.toUpperCase()"></select-date>
                    </v-col>
                  </v-row>
                  {{ editedItem }}
                </v-container>
              </v-card-text>
  
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  color="yellow darken-1"
                  icon
                  fab
                  @click="close"
                >
                  <v-icon large>
                    mdi-restore
                  </v-icon>
                </v-btn>
                <v-btn
                  color="green darken-1"
                  icon
                  fab
                  @click="save"
                >
                  <v-icon large>
                    mdi-content-save
                  </v-icon>
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="text-h5">Silmek istediğinize emin misiniz?</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="yellow darken-1" text @click="closeDelete">
                  <v-icon large>
                    mdi-restore
                  </v-icon>
                </v-btn>
                <v-btn color="red darken-1" text @click="deleteItemConfirm">
                  <v-icon large>
                    mdi-content-save
                  </v-icon>
                </v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
         </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:item.actions="{ item }">
        <v-icon
          color="blue"
          class="mr-2"
          @click="editItem(item)"
        >
          mdi-pencil
        </v-icon>
        <v-icon
          color="red"
          @click="deleteItem(item)"
        >
          mdi-delete
        </v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn
          color="primary"
          @click=""
        >
          TEMİZLE
        </v-btn>
      </template>
    </v-data-table>
  </template>
  <script>
  export default {
    name: 'CrudDataTable',

    data: () => ({
      dialog: false,
      dialogDelete: false,
      editedIndex: -1,  
    }),
  
    computed: {
      formTitle() {
        return this.editedIndex === -1 ? `Yeni ${this.formTitleText}` : `Düzenle ${this.formTitleText}`
      },
    },
  
    watch: {
      dialog(val) {
        val || this.close()
      },
      dialogDelete(val) {
        val || this.closeDelete()
      },
    },
  
    created() {
    },
  
    methods: {
      editItem(item) {
        this.editedIndex = this.items.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },
  
      deleteItem(item) {
        this.editedIndex = this.items.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialogDelete = true
      },
  
      deleteItemConfirm() {
        this.items.splice(this.editedIndex, 1)
        this.closeDelete()
      },
  
      close() {
        this.dialog = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },
  
      closeDelete() {
        this.dialogDelete = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },
  
      save() {
        if (this.editedIndex > -1) {
          Object.assign(this.items[this.editedIndex], this.editedItem)
        } else {
          this.$axios.post(this.postUrl, this.editedItem)
            .then(res => {
              this.items.push(res.data)
            })
            .catch(err => console.error(err))
        }
        this.close()
      },
    },
  }
  </script>