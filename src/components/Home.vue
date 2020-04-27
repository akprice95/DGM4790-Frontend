<template>
  <!-- Apollo watched Graphql query -->
  <ApolloQuery :query="require('../graphql/AllStats.gql')" :variables="{searchString}">
    <template v-slot="{result: {loading, error, data}}">
      <!-- Loading -->
      <div v-if="loading" class="loading apollo">Loading...</div>

      <!-- Error -->
      <div v-else-if="error" class="error apollo">An error occured</div>

      <!-- Result -->
      <div v-else-if="data" class="result-apollo">
        <v-row>
          <v-col cols="4" v-for="(item, i) in data.Stats" :key="i">
            <v-card class="mx-auto" max-width="550">
              <v-card-text>
                <p class="statsStyle">ID: {{ item.id }}</p>
                <p class="statsStyle">Name: {{ item.name }}</p>
                <p class="statsStyle">GamerTag: {{ item.GamerTag }}</p>
                <p class="statsStyle">Platform: {{ item.Platform }}</p>
                <p class="statsStyle">KD: {{ item.KD }}</p>
                <p class="statsStyle">updated At: {{ item.updatedAt }}</p>
              </v-card-text>
              <v-card-actions>
                <v-btn color="primary" fab x-small dark @click="editCourse(item)">
                  <v-icon>mdi-pencil</v-icon>
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </div>
      <!-- No result -->
      <div v-else class="no-result apollo">No result :(</div>
    </template>
  </ApolloQuery>
</template>

<script>
//import vue-truncate-filter from 'vue-truncate-filter'

export default {
  name: "Home",

  data: () => ({
    searchString: "DGM"
  }),
  methods: {
    editCourse(course) {
      this.$store.dispatch("editCourse", course);
      this.$router.push("admin");
    }
  }
};
</script>

<style scoped>
.result {
  padding: 1rem;
}

.statsStyle {
  font-size: 1.25rem;
  font-weight: 600;
  color: black;
  text-transform: uppercase;
  font-weight: bold;
  text-align: center;
}

.mx-auto {
  background: #09eae2;
}

.result-apollo {
  background: black;
}
</style>
