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
          <v-col col v-for="(item, i) in data.Stats" :key="i">
            <v-card class="mx-auto" width="450">
              <v-card-text>
                <p class="statsStyle">ID: {{ item.id }}</p>

                <p class="statsStyle">
                  <span class="title">Platform:</span>
                  {{ item.Platform }}
                </p>
                <p class="statsStyle">
                  <span class="title">KD:</span>
                  {{ item.KD }}
                </p>
                <p class="statsStyle">
                  <span class="title">updated At:</span>
                  {{ item.updatedAt }}
                </p>
              </v-card-text>
              <!-- add this div and put name and gamer tag in item
              give this div its own classname mx-auto-name-->
              <div class="mx-auto-name" id="test">
                <p class="statsStyle bold">
                  <span class="title">Name:</span>
                  {{ item.name }}
                </p>
                <p class="statsStyle">
                  <span class="title">GamerTag:</span>
                  {{ item.GamerTag }}
                </p>
              </div>
              <v-card-actions class="mx-auto-actions">
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
}

.title {
  font-weight: bold;
  text-transform: uppercase;
}

.bold {
  font-size: 1.5em;
}

.mx-auto {
  background: #76b6cc;
  box-shadow: 0 10px 12px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
  text-align: left;
}

.mx-auto:hover {
  box-shadow: 12px 16px 10px rgba(0, 0, 0, 0.2);
  background: #49869c;
}

.mx-auto-name {
  width: 100%;
  height: 100px;
  background: white;
  text-align: left;
  padding: 10px 15px;
}

.mx-auto-actions {
  background: white;
}

.result-apollo {
  /* background: black; */
}
</style>
