<template>
  <q-page padding>
    <div v-if="!utilisateur">
      <h6>Formulaire de connexion</h6>
      <q-form @submit="connexion" class="q-gutter-md">
        <q-input v-model="email" label="E-mail" />
        <q-input v-model="mdp" type="password" label="Mot de passe" />
        <q-btn type="submit" label="Se connecter" color="primary"/>
      </q-form>
    </div>
    <div v-else>
      <p>
        {{ utilisateur.name }}
        <q-btn label="Se déconnecter" @click="deconnexion"/>
      </p>
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
    </div>
  </q-page>
</template>

<script>
export default {
  name: 'PageTaches',
  data () {
    return {
      email: 'user1@gmail.com',
      mdp: '12345',
      utilisateur: null,
      taches: []
    }
  },
  methods: {
    connexion () {
      const thisComp = this
      this.$api.post('login', { email: this.email, password: this.mdp })
        .then(function (reponse) {
          // Récupère l'utilisateur de la réponse et le stoque dans les datas
          thisComp.utilisateur = reponse.data.user
          // Récupère le token de la réponse et le stoque dans les datas
          thisComp.utilisateur.token = reponse.data.access_token
          // Récupère les tâches de l'utilisateur
          thisComp.getTaches()
          // Sauvegarde l'utilisateur dans le "cache" du navigateur, le local data stograge
          thisComp.$q.localStorage.set('utilisateur', thisComp.utilisateur)
        })
        .catch(function (error) {
          alert(error.response.data.message)
          throw error
        })
    },
    deconnexion () {
      const thisComp = this
      // Entête de configuration pour passer le token à l'API
      const config = {
        headers: { Authorization: 'Bearer ' + thisComp.utilisateur.token }
      }
      this.$api.post('logout', {}, config)
        .then(function () {
          // Supprime l'utilisateur et ses tâches
          thisComp.utilisateur = null
          thisComp.taches = []
        })
    },
    getTaches () {
      // Récupère la référence de notre composant
      const thisComp = this
      // Token d'identification
      const token = thisComp.utilisateur.token
      // Entête de configuration pour passer le token à l'API
      const config = {
        headers: { Authorization: 'Bearer ' + token }
      }
      // Récupération de la liste des tâches depuis l'API
      this.$api.get('taches', config)
        .then(function (reponse) {
          console.log(reponse.data)
          thisComp.taches = reponse.data
        })
    }
  },
  // Mounted se déclenche juste avant l'affichage du composant
  mounted () {
    // Récupère l'utilisateur dans le local storage
    const localStorageUser = this.$q.localStorage.getItem('utilisateur')
    // Si un utilisateur a été trouvé on le charge dans les data
    if (localStorageUser) {
      this.utilisateur = localStorageUser
      this.getTaches()
    }
  }
}
</script>

<style scoped lang="sass">
.text-barre
  text-decoration: line-through
</style>
