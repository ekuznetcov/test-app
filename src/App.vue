<template>
  <v-app>
    <v-main>
      <v-banner
        sticky
        shaped
      >
        <v-btn
          fab
          dark
          absolute
          right
          large
          color="deep-purple accent-3"
          @click="dialog=true"
        >
          <v-icon>mdi-plus</v-icon>
        </v-btn>
      </v-banner>
      <v-container>
        <v-dialog
          v-model="dialog"
          fullscreen
          hide-overlay
          persistent
          transition="dialog-bottom-transition"
        >
          <v-card>
            <v-toolbar
              dark
              color="deep-purple accent-3"
            >
              <v-btn
                icon
                dark
                @click="dialog = false"
              >
                <v-icon>mdi-close</v-icon>
              </v-btn>
              <v-toolbar-title>Set Items</v-toolbar-title>
              <v-spacer></v-spacer>
              <v-toolbar-items>
                <v-btn
                  dark
                  text
                  @click="saveItem"
                >
                  Save
                </v-btn>
              </v-toolbar-items>
            </v-toolbar>
            <v-row>
              <v-col cols="12"
                sm="6"
                md="4"
                lg="3"
              >
                <v-color-picker class="ma-5" v-model="color"></v-color-picker>
              </v-col>
              <v-col cols="12"
                md="6"
                lg="6"
              >
                <v-text-field
                  v-model="itemName"
                  label="Name"
                  clearable
                  hide-details="auto"
                ></v-text-field>
                <v-text-field class="mb-5"
                  v-model="itemType"
                  label="Type"
                  clearable
                  hide-details="auto"
                >
                </v-text-field>
              </v-col>
            </v-row>
          </v-card>
        </v-dialog>
      <data-table
        :items="items"
        @deleteItem="deleteItem"
        @updateItem="updateItem"
      />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import DataTable from './components/DataTable.vue';
import {v4 as uuidv4} from 'uuid'


export default {  
  name: 'App',

  components: {
    DataTable
  },

  data: () => ({
      types: ['hex', 'hexa', 'rgba', 'hsla', 'hsva'],
      type: 'hex',
      hex: '#FF00FF',
      hexa: '#FF00FFFF',
      rgba: { r: 255, g: 0, b: 255, a: 1 },
      hsla: { h: 300, s: 1, l: 0.5, a: 1 },
      hsva: { h: 300, s: 1, v: 1, a: 1 },
      itemName: '',
      itemType: '',
      temp: [],
      dialog: false,
  }),

  computed: {
      color: {
        get () {
          return this[this.type]
        },
        set (value) {
          this[this.type] = value
        },
      },
      
      items(){
        try{
          let temp = this.temp
          let items = localStorage.getItem('items')
          if (items === null || items === undefined)
            items = []
          console.log( JSON.parse(items) )
          return JSON.parse(items)
        }catch(err){
          console.error('Не удалось получить достать данные')
          return []
        }
      },
  },

  methods:{
    saveItem(){
      if(this.itemType.length == 0 || this.itemName == 0){
        this.$emit('empty-input')
        return
      }
      let items = this.items
      let newItem = {
        id: uuidv4(),
        name: this.itemName,
        type: this.itemType,
        color: this.color
      }
      items.push(newItem)
      this.temp = items
      this.dialog = false
      localStorage.setItem('items', JSON.stringify(items))
    },

    updateItem(index){
      let items = this.items
      this.temp = items
      this.itemName = items[index].name
      this.itemType = items[index].type
      this.hex = items[index].color
      this.dialog = true
    },

    deleteItem(id){
      let items = this.items
      let index
      for (let i in items)
        if(items[i].id == id)
          index= i
      items.splice(index,index+1)
      this.temp= items
      localStorage.setItem('items',JSON.stringify(items))
    },

  },
};
</script>
