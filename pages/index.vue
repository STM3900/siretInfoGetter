<template>
  <div>
    <h1>siretInfoGetter</h1>
    <p>Salut {{ name }}</p>
    <button @click="getCompanySiret">Recherche par SIRET</button>
    <button @click="getCompanySiren">Recherche par SIREN</button>
    <p>Données de la réponse API : {{ data }}</p>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      name: "Théo",
      data: "",
    };
  },
  methods: {
    getCompanySiret() {
      const res = this.$axios
        .get(
          "https://entreprise.data.gouv.fr/api/sirene/v1/siret/21310555400017"
        )
        .then((response) => {
          this.data = response;
        });
    },
    getCompanySiren() {
      const res = this.$axios
        .get("https://entreprise.data.gouv.fr/api/sirene/v1/siren/213105554")
        .then((response) => {
          this.data = response;
        });
    },
  },
};
</script>
