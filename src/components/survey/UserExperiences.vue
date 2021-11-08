<template>
  <section>
    <base-card>
      <h2>Submitted Experiences</h2>
      <div>
        <base-button @click="loadExperience"
          >Load Submitted Experiences</base-button
        >
      </div>
      <p v-if="isLoading">Loading...</p>
      <p v-else-if="!isLoading && (!results || results.length === 0)">
        Nothis is found
      </p>
      <ul v-else-if="!isLoading && results && results.length > 0">
        <survey-result
          v-for="result in results"
          :key="result.id"
          :name="result.name"
          :rating="result.rating"
        ></survey-result>
      </ul>
    </base-card>
  </section>
</template>

<script>
import SurveyResult from './SurveyResult.vue';

export default {
  components: {
    SurveyResult,
  },
  data() {
    return {
      results: [],
      isLoading: true,
    };
  },
  methods: {
    // loadExperience() {
    //   fetch(
    //     'https://vue-http-demo-f70f3-default-rtdb.firebaseio.com/surveys.json'
    //   )
    //     .then((res) => {
    //       if (res.ok) {
    //         return res.json();
    //       }
    //     })
    //     .then((data) => {
    //       const results = [];
    //       for (let id in data) {
    //         results.push({
    //           id: id,
    //           name: data[id].name,
    //           rating: data[id].rating,
    //         });
    //       }

    //       this.results = results;
    //     });
    // },

    async loadExperience() {
      this.isLoading = false;
      const res = await fetch(
        'https://vue-http-demo-f70f3-default-rtdb.firebaseio.com/surveys.json'
      );
      const data = await res.json();
      const results = [];
      for (let id in data) {
        results.push({
          id: id,
          name: data[id].name,
          rating: data[id].rating,
        });
      }

      this.results = results;
    },
  },
  mounted() {
    this.loadExperience();
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}
</style>
