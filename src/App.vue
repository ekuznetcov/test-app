<template>
  <v-app>

    <v-main>
      <v-container>
      <v-row>
      <v-col cols="12"
        sm="6"
        md="4"
        lg="3"
      >
        <v-color-picker v-model="color"></v-color-picker>
      </v-col>
      <v-col cols="12"
        md="6"
        lg="6"
      >
        <v-text-field
          v-model="itemName"
          label="Name"
          :rules="rules"
          clearable
          hide-details="auto"
        ></v-text-field>
        <v-text-field class="mb-5"
          v-model="itemType"
          label="Type"
          :rules="rules"
          clearable
          hide-details="auto"
        >
        </v-text-field>
        <v-btn
          @click="saveItems"
        >
          Save
        </v-btn>
      </v-col>
      </v-row>
      <data-table
        :items="items"
        @deleteItem="deleteItem"
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
      rules: [
        value => !!value || 'Required.',
        value => (value && value.length >= 3) || 'Min 3 characters',
      ],
      itemName: '',
      itemType: '',
      temp: [],
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

      showColor() {
        if (typeof this.color === 'string') return this.color

        return JSON.stringify(Object.keys(this.color).reduce((color, key) => {
          color[key] = Number(this.color[key].toFixed(2))
          return color
        }, {}), null, 2)
      },     
  },

  methods:{
    saveItems(){
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
        localStorage.setItem('items', JSON.stringify(items) )
    },

    deleteItem(i){
      let items = this.items
      items.splice(i,i+1)
      this.temp= items
      localStorage.setItem('items',JSON.stringify(items))
    }
  },
};
</script>
