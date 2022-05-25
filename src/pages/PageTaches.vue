<template>
  <q-page padding>
    <h3 v-if="taches.length === 0">Pas de tâches pour le moment...</h3>
    <q-list v-else separator bordered>
      <q-item
        v-for="tache in taches"
        :key="tache.id"
        @click="tache.terminee = !tache.terminee"
        clickable
        v-ripple
        :class="tache.terminee ? 'bg-green-1' : 'bg-orange-1'"
      >
        <q-item-section side>
          <q-checkbox v-model="tache.terminee" />
        </q-item-section>

        <q-item-section>
          <q-item-label
            :class="{ 'text-barre': tache.terminee }"
          >{{ tache.nom }}</q-item-label>
        </q-item-section>

        <q-item-section side>
          <!-- Création d'une ligne Flex -->
          <div class="row">
            <!-- Création d'une colonne Flex -->
            <div class="column justify-center">
              <q-icon
                size="18px"
                name="event"
                class="q-mr-xs"
              />
            </div>
            <!-- Création d'une colonne Flex -->
            <div class="column">
              <q-item-label
                class="text-right"
                caption
              >
                {{ tache.dateFin }}
              </q-item-label>
              <q-item-label
                class="text-right"
                caption
              >
                <small>{{ tache.heureFin }}</small>
              </q-item-label>
            </div>
          </div>
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
export default {
  name: 'PageTaches',
  data () {
    return {
      taches: []
    }
  }
}
</script>

<style scoped lang="sass">
.text-barre
  text-decoration: line-through
</style>
