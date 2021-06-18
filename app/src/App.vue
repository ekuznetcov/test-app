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
          @click="callDialog('new')"
        >
          <v-icon>mdi-plus</v-icon>
        </v-btn>
      </v-banner>
      <Dialog
        :itemName.sync = "itemName"
        :itemType.sync = "itemType"
        :hex.sync = "color"
        :dialog.sync="dialog"
        :type="updateItemId"
        @updateItem="updateItem"
        @saveItem="saveItem"
      />
      <v-container>
        <data-table
          :items="items"
          @deleteItem = "deleteItem"
          @callDialog = "callDialog"
        />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import {v4 as uuidv4} from 'uuid'
import DataTable from './components/DataTable.vue';
import Dialog from './components/Dialog.vue'


export default {  
  name: 'App',

  components: {
    DataTable, Dialog
  },

  data: () => ({
      color: '',
      itemName: '',
      itemType: '',
      temp: [],
      dialog: false,
      updateItemId: 'new',
  }),

  computed: {     
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
    deleteItem(itemId){
      let items = this.items
      let index = this.findeIndexById(itemId)
      items.splice(index,index+1)
      this.temp= items
      localStorage.setItem('items',JSON.stringify(items))
    },

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

    updateItem(itemId){
      if(this.itemType.length == 0 || this.itemName == 0){
        this.$emit('empty-input')
        return
      }
      let index = this.findeIndexById(itemId)
      let item = this.items[index]
      item.name = this.itemName
      item.type = this.itemType
      item.color = this.color
      this.dialog = false
      localStorage.setItem('items', JSON.stringify(this.items))
    },

    callDialog(itemId){
      if(itemId=='new')
        this.setup('','','')
      else{
        let items = this.items
        this.temp = items
        let index = this.findeIndexById(itemId)
        let item = items[index]
        this.setup(item.name, item.type, item.color)
      }
      this.updateItemId = itemId
      this.dialog = true
    },

    setup(name, type, color){
      this.itemName = name
      this.itemType = type
      this.color = color
    },

    findeIndexById(itemId){
      for (let index in this.items)
        if(this.items[index].id == itemId)
          return index
      return -1
    },

  },
};
</script>
