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
          Quasar App
        </q-toolbar-title>

        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
    >
      <q-list>
        <q-list>
          <q-item-label header>
            Menu de navigation
          </q-item-label>

          <q-item
            v-for="lien in liens"
            :key="lien.id"
            :to="lien.route"
            exact
            clickable
          >
            <q-item-section avatar>
              <q-icon :name="lien.icone" />
            </q-item-section>

            <q-item-section>
              <q-item-label>{{ lien.libelle }}</q-item-label>
            </q-item-section>
          </q-item>

        </q-list>
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>

    <q-footer elevated>
      <q-tabs>
        <q-route-tab
          v-for="lien in liens"
          :key="lien.id"
          :to="lien.route"
          :icon="lien.icone"
          :label="lien.libelle"
          exact
        />
      </q-tabs>
    </q-footer>
  </q-layout>
</template>

<script>

export default {
  name: 'MainLayout',
  data () {
    return {
      leftDrawerOpen: false,
      liens: [ // Tableau des liens de l'application
        {
          id: 1,
          libelle: 'Tâches',
          icone: 'list',
          route: '/'
        },
        {
          id: 2,
          libelle: 'Paramètres',
          icone: 'settings',
          route: '/params'
        }
      ]
    }
  },
  methods: {
    toggleLeftDrawer () {
      this.leftDrawerOpen = !this.leftDrawerOpen
    }
  }
}
</script>
