<template>
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
          </v-col>
        </v-row>
      </v-card>
    </v-dialog>
</template>

<script>

export default {
    props:{
      dialog: Boolean
    }
}
</script>
