<template>
  <q-page padding>
    <q-list separator bordered>
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
                <small>{{ tache.heureFin}}</small>
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
      taches: [
        {
          id: 1,
          nom: 'Acheter des oranges',
          terminee: false,
          dateFin: '06.06.2020',
          heureFin: '12:00'
        },
        {
          id: 2,
          nom: 'Manger des oranges',
          terminee: false,
          dateFin: '15.06.2020',
          heureFin: '22:00'
        },
        {
          id: 3,
          nom: 'Digérer des oranges',
          terminee: false,
          dateFin: '16.06.2020',
          heureFin: '14:00'
        }
      ]
    }
  }
}
</script>

<style scoped lang="sass">
.text-barre
  text-decoration: line-through
</style>
