<template>
  <div class="page">
    <v-container>
      <v-row>
        <v-col>
          <h1>GraphQl CRUD</h1>
          <v-card-title>Get an Item by ID</v-card-title>
          <v-text-field v-model="idInput" label="Enter Item ID..."></v-text-field>
        </v-col>
      </v-row>
      <!-- Apollo watched Graphql query -->
      <ApolloQuery :query="require('../graphql/getStat.gql')" :variables="{ idInput }">
        <template v-slot="{ result: { loading, error, data } }">
          <!-- Loading -->
          <div v-if="loading" class="loading apollo">Loading...</div>

          <!-- Error -->
          <v-alert type="error" v-else-if="error" class="error apollo">An error occured</v-alert>

          <!-- Result -->
          <div v-else-if="data" class="result apollo">
            <v-row>
              <v-col cols="3" v-for="(item, i) in data" :key="i">
                <v-card class="mx-auto" max-width="350">
                  <v-card-text>
                    <p>ID: {{ item.id }}</p>
                    <p>Updated At: {{ item.updatedAt }}</p>
                    <p>
                      Name:
                      <strong>{{ item.name }}</strong>
                    </p>
                    <p>KD: {{ item.KD }}</p>
                    <p>GamerTag: {{ item.GamerTag }}</p>
                    <p>Platform: {{ item.Platform }}</p>
                  </v-card-text>
                </v-card>
              </v-col>
            </v-row>
          </div>

          <!-- No result -->
          <div v-else class="no-result apollo">No result :(</div>
        </template>
      </ApolloQuery>

      <v-row>
        <v-col>
          <v-card-title>Search all items</v-card-title>
          <v-text-field v-model="searchString" label="Search for an item..."></v-text-field>
        </v-col>
      </v-row>
      <!-- Apollo watched Graphql query -->
      <ApolloQuery :query="require('../graphql/SearchGamer.gql')" :variables="{ searchString }">
        <template v-slot="{ result: { loading, error, data } }">
          <!-- Loading -->
          <div v-if="loading" class="loading apollo">Loading...</div>

          <!-- Error -->
          <v-alert type="error" v-else-if="error" class="error apollo">An error occured</v-alert>

          <!-- Result -->
          <div v-else-if="data" class="result apollo">
            <v-row>
              <v-col cols="3" v-for="(item, i) in data.filterStat" :key="i">
                <v-card class="mx-auto" max-width="350">
                  <v-card-text>
                    <p>Name: {{ item.name }}</p>
                    <p>
                      KD:
                      <strong>{{ item.KD }}</strong>
                    </p>
                    <p>GamerTag: {{ item.GamerTag }}</p>
                    <p>Platform: {{ item.Platform }}</p>
                  </v-card-text>
                </v-card>
              </v-col>
            </v-row>
          </div>
          <!-- No result -->
          <div v-else class="no-result apollo">No result :(</div>
        </template>
      </ApolloQuery>
    </v-container>

    <!-- CREATE ITEM -->
    <ApolloMutation
      :mutation="require('../graphql/createStats.gql')"
      :variables="{
      createName,
      createGamerTag,
      createKD,
      createPlatform,
      }"
      @done="onDone"
    >
      <template v-slot="{ mutate, loading, error }">
        <!-- Form here -->
        <v-form>
          <v-container>
            <v-card-title>Create an Item</v-card-title>
            <v-row>
              <v-col cols="6" md="6">
                <v-text-field v-model="createName" label="Name" required filled></v-text-field>
              </v-col>
              <v-col cols="6" md="6">
                <v-text-field v-model="createGamerTag" label="GamerTag" rows="4" required filled></v-text-field>
              </v-col>
              <v-col cols="6" md="6">
                <v-text-field v-model="createKD" label="KD" rows="4" required filled></v-text-field>
              </v-col>
              <v-col cols="6" md="6">
                <v-text-field
                  v-model="createPlatform"
                  label="createPlatform"
                  rows="4"
                  required
                  filled
                ></v-text-field>
              </v-col>
            </v-row>
            <v-btn large class="button" :disabled="loading" @click="mutate()">Add Item</v-btn>
            <p v-if="error">An error occurred: {{ error }}</p>
          </v-container>
        </v-form>
      </template>
    </ApolloMutation>

    <!-- UPDATE Gamer -->
    <ApolloMutation
      :mutation="require('../graphql/updateStats.gql')"
      :variables="{
      id,
      name,
      GamerTag,
      KD,
      Platform
        }"
      @done="onDone"
    >
      <template v-slot="{ mutate, loading, error }">
        <!-- Form here -->
        <v-form color="#ffffff">
          <v-container>
            <v-card-title>Update Gamer</v-card-title>
            <v-row>
              <v-col cols="4" md="4">
                <v-text-field v-model="id" label="id" required filled></v-text-field>
              </v-col>
              <v-col cols="4" md="4">
                <v-text-field v-model="name" label="name" required filled></v-text-field>
              </v-col>
              <v-col cols="4" md="4">
                <v-text-field v-model="GamerTag" label="GamerTag" required filled></v-text-field>
              </v-col>
              <v-col cols="4" md="4">
                <v-text-field v-model="KD" label="KD" required filled></v-text-field>
              </v-col>
              <v-col cols="4" md="4">
                <v-text-field v-model="Platform" label="Platform" required filled></v-text-field>
              </v-col>
            </v-row>

            <v-btn large class="button" :disabled="loading" @click="mutate()">Update Item</v-btn>
            <p v-if="error">An error occurred: {{ error }}</p>
          </v-container>
        </v-form>
      </template>
    </ApolloMutation>

    <!-- DELETE PERSON -->
    <ApolloMutation
      :mutation="require('../graphql/DeleteOne.gql')"
      :variables="{
        deleteOne,
        }"
      @done="onDone"
    >
      <template v-slot="{ mutate, loading, error }">
        <!-- Form here -->
        <v-form>
          <v-container>
            <v-card-title>Delete an Item</v-card-title>
            <v-row>
              <v-col cols="12" md="12">
                <v-text-field class="form" v-model="deleteOne" label="id" required filled></v-text-field>
              </v-col>
            </v-row>

            <v-btn large class="button" :disabled="loading" @click="mutate()">Delete Gamer</v-btn>
            <p v-if="error">An error occurred: {{ error }}</p>
          </v-container>
        </v-form>
      </template>
    </ApolloMutation>
  </div>
</template>
  

<script>
export default {
  data: () => ({
    idInput: "",
    updatedAt: "",
    name: "",
    KD: "",
    GamerTag: "",
    Platform: "",
    id: "",
    searchString: "akprice95",
    createName: "",
    createGamerTag: "",
    createKD: "",
    createPlatform: "",
    deleteOne: ""
  }),
  methods: {
    onDone() {
      return console.log("Done" + "did it work?");
    }
  }
};
</script>

<style scoped>
/* .v-input {
  background: black;
  color: white;
}

.v-text-field__slot {
  color: white;
} */
</style>