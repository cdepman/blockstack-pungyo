<template>
  <v-dialog
    v-model="showAddFriendDialog"
    max-width="500px"
    dark
    persistent
  >
    <v-card>
      <v-card-title style="justify-content: space-between;">
        <span class="headline">
          {{ formTitle }}
        </span>
        <v-btn
          dark
          medium
          color="purple darken-3"
          v-on:click="$emit('scanQR')"
        >
        <i class="action-icon fas fa-camera fa-2x" id="camera_icon"></i>
        Scan QR
      </v-btn>
      </v-card-title>

      <v-card-text>
        <v-layout justify-center column fill-height>
            <v-text-field
              :rules="['Required']"
              v-model="person.name"
              label="Name"
            ></v-text-field>
            <div v-for="note in person.notes" v-bind:key="note.id">
              <v-text-field
                v-model="note.title"
                label="Note Title"
              ></v-text-field>
              <v-text-field
                v-model="note.content"
                label="Note Content"
              ></v-text-field>
            </div>
        </v-layout>
      </v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn
          color="blue darken-1"
          flat
          @click="$emit('hideAddFriendDialog')"
        >
          Cancel
        </v-btn>
        <v-btn
          color="blue darken-1"
          flat
          @click="addFriend"
        >
          Save
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
<script>
export default {
  data () {
    return {
      formTitle: 'Add Friend'
    }
  },
  props: {
    showAddFriendDialog: Boolean,
    person: Object
  },
  methods: {
    addFriend () {
      if (this.inComplete()) { return }
      this.$emit('addPerson', this.person)
      this.$emit('hideAddFriendDialog')
    },
    inComplete () {
      const firstNote = this.person.notes[0]
      if (!this.person.name || !firstNote.title || !firstNote.content) {
        alert('Please enter a name and at least one note with a title!')
        return true
      }
    }
  }
}
</script>
<style scoped>
#camera_icon {
  padding-right: 10px;
}
</style>
