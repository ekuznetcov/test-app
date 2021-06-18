<template>
    <v-dialog
      v-model="dialog"
      fullscreen
      hide-overlay
      persistent
      transition="dialog-bottom-transition"
    >
          {{color}}

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
              v-if="type=='new'"
              @click="$emit('saveItem', type)"
              dark
              text
            >
              Save
            </v-btn>
            <v-btn
              v-else
              @click="$emit('updateItem', type)"
              dark
              text
            >
              Update
            </v-btn>
          </v-toolbar-items>
        </v-toolbar>
        <v-row>
          <v-col cols="12"
            sm="6" md="4" lg="3"
          >
            <v-color-picker class="ma-5" v-model="color"></v-color-picker>
          </v-col>
          <v-col cols="12"
            md="6" lg="6"
          >
            <v-text-field
              v-model="newName"
              label="Name"
              clearable
              hide-details="auto"
            ></v-text-field>
            <v-text-field class="mb-5"
              v-model="newType"
              label="Type"
              clearable
              hide-details="auto"
            >
            </v-text-field>
          </v-col>
        </v-row>
      </v-card>
    </v-dialog>
</template>

<script>
export default {
    data: ()=>({
      mode: 'hex',
      modes: ['hsl', 'rgb', 'hex'],
      rgb: { r: 255, g: 0, b: 255 },
      hsl: { h: 300, s: 1, l: 0.5},
    }),

    props:{
      dialog: Boolean,
      type: String,
      itemName: String,
      itemType: String,
      hex: String
    },

    computed:{
      color: {
        get () {
          return this[this.mode]
        },
        set (value) {
          console.log(value)
          this.$emit('update:hex',value)
        },
      },

      newName: {
        get () {
          return this.itemName
        },
        set (value) {
          this.$emit('update:itemName', value)
        },
      },

      newType: {
        get () {
          return this.itemType
        },
        set (value) {
          this.$emit('update:itemType', value)

        },
      },
    }
}
</script>
