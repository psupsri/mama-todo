<template>
  <v-container fill-height>
    <v-layout justify-center>
      <v-flex xs6>
        <h3 class="display-3">Mama Todos</h3>
        <v-alert
          :value="error"
          color="error"
          icon="warning"
          outline
        >
          Please input something.
        </v-alert>
        <v-text-field
          solo
          label="What needs to be done?"
          append-icon="keyboard"
          v-on:keyup.13="submit"
          v-model="input"
        ></v-text-field>
        <v-list subheader two-line class="elevation-3">
          <v-subheader>
            {{ showItems.length }} items
            <v-spacer></v-spacer>
            <div class="text-xs-center">
              <v-chip outline color="primary"
                :selected="filter === 'all'"
                @click="toggleFilter('all')">All</v-chip>
              <v-chip outline color="warning"
                @click="toggleFilter('active')">Active</v-chip>
              <v-chip outline color="success"
                @click="toggleFilter('complete')">Complete</v-chip>
            </div>
            <v-spacer></v-spacer>
            <v-btn flat color="error"
              @click="clear">
              clear
            </v-btn>
          </v-subheader>
          <template v-for="(item, index) in showItems">
            <v-divider :key="`divider-` + index"></v-divider>
            <v-list-tile
              :key="`tile-` + index"
            >
              <v-list-tile-content>
                <v-list-tile-title
                  :class="{'grey--text font-italic complete': item.status === true}"
                  v-html="item.title">
                </v-list-tile-title>
              </v-list-tile-content>

              <v-list-tile-action
                :key="`complete-` + index"
                @click="complete(index)"
                v-if="item.status === false">
                <v-btn icon>
                  <v-icon color="success">done</v-icon>
                </v-btn>
              </v-list-tile-action>

              <v-list-tile-action
                :key="`del-` + index"
                @click="del(index)">
                <v-btn icon>
                  <v-icon color="error">clear</v-icon>
                </v-btn>
              </v-list-tile-action>

            </v-list-tile>
          </template>
          <v-divider></v-divider>
          <v-list-tile style="height: 32px">
          </v-list-tile>
        </v-list>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import _ from 'lodash'

export default {
  data: () => ({
    items: [
      { title: 'exam', status: false }
    ],
    input: '',
    error: false,
    filter: 'all'
  }),
  computed: {
    showItems () {
      if (this.filter === 'all') {
        return this.items
      } else if (this.filter === 'active') {
        return _.filter(this.items, { 'status': false })
      } else {
        return _.filter(this.items, 'status')
      }
    }
  },
  methods: {
    submit () {
      if (this.input === '') {
        this.error = true
        return
      }
      this.items.unshift({
        title: this.input,
        status: false
      })
      this.input = ''
    },
    toggleFilter (v) {
      this.filter = v
    },
    complete (v) {
      this.items[v].status = true
    },
    del (v) {
      this.items.splice(v, 1)
    },
    clear () {
      this.items = []
    }
  }
}
</script>

<style scoped>
.complete {
  text-decoration: line-through;
}
</style>
