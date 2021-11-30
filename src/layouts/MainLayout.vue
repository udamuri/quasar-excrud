<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title>
          Petani APP
        </q-toolbar-title>

        <div></div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
    >
     <q-scroll-area style="height: calc(100%);border-right: 1px solid #ddd">
        <div class="q-pa-md q-gutter-md">
          <q-list>
            <q-item-label header>Folders</q-item-label>

            <q-item
              clickable
              v-ripple
              :active="'/'"
              to = "/"
              active-class="my-menu-link"
            >
              <q-item-section avatar top>
                <q-avatar icon="folder" color="primary" text-color="white" />
              </q-item-section>

              <q-item-section>
                <q-item-label lines="1">Dashboard</q-item-label>
              </q-item-section>

              <q-item-section side>
                <q-icon name="info" color="green" />
              </q-item-section>
            </q-item>

            

            <q-separator spaced />
            <q-item-label header>Files</q-item-label>

            <q-item clickable v-ripple>
              <q-item-section avatar top>
                <q-avatar icon="assignment" color="grey" text-color="white" />
              </q-item-section>

              <q-item-section>
                <q-item-label lines="1">Expenses spreadsheet</q-item-label>
              </q-item-section>

              <q-item-section side>
                <q-icon name="info" />
              </q-item-section>
            </q-item>

          </q-list>
        </div>
      </q-scroll-area>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'MainLayout',

  components: {},

  setup () {
    const leftDrawerOpen = ref(false)

    return {
      leftDrawerOpen,
      toggleLeftDrawer () {
        leftDrawerOpen.value = !leftDrawerOpen.value
      }
    }
  }
})
</script>
