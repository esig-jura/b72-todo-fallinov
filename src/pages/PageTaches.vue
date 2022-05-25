<template>
  <q-page padding>
    <h1>Tâches</h1>
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
          terminee: false
        },
        {
          id: 2,
          nom: 'Manger des oranges',
          terminee: false
        },
        {
          id: 3,
          nom: 'Digérer des oranges',
          terminee: false
        }
      ]
    }
  }
}
</script>

<style scoped>

</style>
