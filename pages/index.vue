<template>
  <div>
    <h1>Siret Info Getter</h1>
    <header>
      <input
        type="text"
        v-model="siret"
        maxlength="14"
        placeholder="Siret de l'entreprise voulu"
        @input="error = false"
      />
      <button @click="getCompanySiret" :disabled="checkLength(siret)">
        Rechercher
      </button>
    </header>

    <div v-if="data" class="data">
      <p>
        Siret : <b>{{ data.siret }}</b>
      </p>
      <p>
        Siren : <b>{{ data.siren }}</b>
      </p>
      <p>
        Raison sociale : <b>{{ data.nom_raison_sociale }}</b>
      </p>
      <p>
        Adresse : <b> {{ data.geo_adresse }}</b>
      </p>
      <p>
        Activité principale :
        <b>{{ data.libelle_activite_principale_entreprise }}</b>
      </p>
      <p>
        Nature juridique : <b>{{ data.libelle_nature_juridique_entreprise }}</b>
      </p>
    </div>
    <div v-if="error">
      <p>Siret inconnu, réessayez</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      name: "Théo",
      data: "",
      siret: "",
      error: false,
    };
  },
  methods: {
    getCompanySiret() {
      this.data = "";
      const res = this.$axios
        .get(
          `https://entreprise.data.gouv.fr/api/sirene/v1/siret/${this.siret}`
        )
        .then((response) => {
          this.error = false;
          this.data = response.data.etablissement;
        })
        .catch((error) => {
          this.error = true;
        });
    },
    checkLength(str) {
      return str.length < 14;
    },
  },
};
</script>

<style>
body {
  font-family: "Open Sans", sans-serif;
  margin-right: 20%;
  margin-left: 20%;
}

header {
  max-width: 725px;
  min-height: 150px;

  transition: 0.3s;
  border: solid 1px #e1e1e1;

  border-radius: 5px;

  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  align-content: center;
  gap: 15px;

  padding: 10px 20px;
}

header input {
  font-family: "Open Sans", sans-serif;

  /* haut | droit | bas | gauche */
  padding: 5px 2px 2px 2px;

  font-size: 16px;
  border: none;
  border-bottom: solid 1px black;

  outline: none;
  width: 50%;
}

header button {
  font-family: "Open Sans", sans-serif;

  background: none;
  transition: 0.3s;
  padding: 8px 15px;

  border-radius: 5px;
  border: none;
  border: solid 1px;
}

header button:disabled {
  opacity: 0.5;
  transform: scale(0.99);
}

header button:not(:active):not(:disabled):hover {
  cursor: pointer;
  transform: scale(0.99);
}

header button:active {
  transform: scale(0.93);
}

.data {
  max-width: 725px;
  min-height: 150px;

  transition: 0.3s;
  border: solid 1px #e1e1e1;

  border-radius: 5px;

  padding: 10px 20px;
  margin-top: 25px;
}

.data p {
  margin: 5px 0px;
}
</style>
