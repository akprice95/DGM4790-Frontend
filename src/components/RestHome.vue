<template>
  <div>
    <v-row>
      <v-container>
        <v-col cols="12">
          <h1>RESTFUL API Home</h1>
          <br />
          <v-btn @click="getData" class="dataBtn" id="getDataBtn">Get All Data</v-btn>
        </v-col>

        <!-- Get all stats -->
        <div>
          <v-row>
            <v-col cols="4" v-for="(item, i) in stats" :key="i">
              <v-card class="mx-auto" max-width="550">
                <v-card-text>
                  <p class="name">ID: {{ item._id }}</p>
                  <p>
                    <strong>KD:</strong>
                    {{ item.KD }}
                  </p>
                  <p>
                    <strong>GamerTag:</strong>
                    {{ item.gamerTag }}
                  </p>
                  <p>
                    <strong>Matches Played:</strong>
                    {{ item.matchesPlayed }}
                  </p>
                  <p>
                    <strong>Platform:</strong>
                    {{ item.platform }}
                  </p>
                  <p>
                    <strong>ID:</strong>
                    {{ item._id }}
                  </p>
                </v-card-text>
              </v-card>
            </v-col>
          </v-row>
        </div>
      </v-container>
    </v-row>
    <!-- Get Specific user -->
    <v-row>
      <v-col cols="12">
        <v-form>
          <v-container>
            <v-card-title>Get a Specific User</v-card-title>
            <v-row>
              <v-col cols="6" md="6">
                <v-text-field v-model="specificID" label="id" required filled></v-text-field>
              </v-col>
            </v-row>
            <v-btn large color="primary" @click="getOneStat()">Get One Specific User</v-btn>
          </v-container>
        </v-form>
        <v-container>
          <div>
            <v-row>
              <v-col cols="3">
                <v-card class="mx-auto" max-width="350">
                  <v-card-text class="cards">
                    <p class="name">
                      <strong>ID:</strong>
                      {{ oneStat._id }}
                    </p>
                    <p>
                      <strong>KD:</strong>
                      {{ oneStat.KD }}
                    </p>
                    <p>
                      <strong>GamerTag:</strong>
                      {{ oneStat.gamerTag }}
                    </p>
                    <p>
                      <strong>Matches Played:</strong>
                      {{ oneStat.matchesPlayed }}
                    </p>
                    <p>
                      <strong>Platform:</strong>
                      {{ oneStat.platform }}
                    </p>
                  </v-card-text>
                </v-card>
              </v-col>
            </v-row>
          </div>
        </v-container>
      </v-col>
    </v-row>

    <!-- Create a Stat -->
    <v-row>
      <v-col cols="12">
        <v-form>
          <v-container>
            <v-card-title>Create a Gamer</v-card-title>
            <v-row>
              <v-col cols="6" md="6">
                <v-text-field v-model="createKD" label="KD" required filled></v-text-field>
              </v-col>
              <v-col cols="6" md="6">
                <v-text-field v-model="createGamerTag" label="Gamer Tag" required filled></v-text-field>
              </v-col>
              <v-col cols="6" md="6">
                <v-text-field v-model="createMatchesPlayed" label="Matches Played" required filled></v-text-field>
              </v-col>
              <v-col cols="6" md="6">
                <v-text-field v-model="createPlatform" label="Platform" required filled></v-text-field>
              </v-col>
            </v-row>
            <v-btn large color="primary" @click="createGamer()">Create Gamer</v-btn>
          </v-container>
        </v-form>
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="12">
        <v-form>
          <v-container>
            <v-card-title>Update an Item</v-card-title>
            <v-row>
              <v-col cols="6" md="6">
                <v-text-field v-model="updateID" label="ID" required filled></v-text-field>
              </v-col>
              <v-col cols="6" md="6">
                <v-text-field v-model="updateKD" label="Update KD?" required filled></v-text-field>
              </v-col>
              <v-col cols="6" md="6">
                <v-text-field v-model="updateGamerTag" label="Update Gamertag?" required filled></v-text-field>
              </v-col>
              <v-col cols="6" md="6">
                <v-text-field
                  v-model="updateMatchesPlayed"
                  label="Update Matches Played?"
                  required
                  filled
                ></v-text-field>
              </v-col>
              <v-col cols="6" md="6">
                <v-text-field v-model="updatePlatform" label="Update Platform?" required filled></v-text-field>
              </v-col>
            </v-row>
            <v-btn large color="primary" @click="EditStat()">Update Gamer</v-btn>
          </v-container>
        </v-form>
      </v-col>
    </v-row>

    <v-row>
      <v-col cols="12">
        <v-form>
          <v-container>
            <v-card-title>Remove a Gamer</v-card-title>
            <v-row>
              <v-col cols="6" md="6">
                <v-text-field v-model="RemoveG" label="ID" required filled></v-text-field>
              </v-col>
            </v-row>
            <v-btn large color="primary" @click="removeGamer()">Remove Gamer</v-btn>
          </v-container>
        </v-form>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "RestHome",

  data: () => ({
    stats: [],
    specificID: [],
    stat: "",
    oneStat: [],
    createKD: "",
    createGamerTag: "",
    createMatchesPlayed: "",
    createPlatform: "",
    updateID: "",
    updateKD: "",
    updateGamerTag: "",
    updateMatchesPlayed: "",
    updatePlatform: ""
  }),
  methods: {
    // GET ALL Stats
    getData() {
      return axios
        .get("http://localhost:5000/admin/getAllStats")
        .then(response => {
          console.log(response);
          this.stats = response.data;
          console.log(JSON.stringify(this.stats));
        })
        .catch(error => console.log(error));
    },

    // Get Specific ID
    getOneStat() {
      console.log(this.specificID);
      const playerId = this.specificID;
      const url = `http://localhost:5000/admin/Stats/` + playerId;
      return axios
        .get(url)
        .then(response => {
          console.log(response);
          this.oneStat = response.data;
        })
        .catch(error => console.log(error));
    },

    // Create a stat
    createGamer() {
      const url = `http://localhost:5000/admin/add-gamer`;
      const data = {
        KD: this.createKD,
        gamerTag: this.createGamerTag,
        matchesPlayed: this.createMatchesPlayed,
        platform: this.createPlatform
      };
      return axios
        .post(url, data)
        .then(response => {
          console.log(response);
          this.stat = response.data;
          console.log(this.stat);
          alert("Gamer Created");
        })
        .catch(error => console.log(error));
    },
    EditStat() {
      const url = `http://localhost:5000/admin/edit-Stats`;
      const data = {
        playerId: this.updateID,
        KD: this.updateKD,
        gamerTag: this.updateGamerTag,
        matchesPlayed: this.updateMatchesPlayed,
        platform: this.updatePlatform
      };
      return axios
        .put(url, data)
        .then(response => {
          console.log(response);
          alert("Gamer updated");
        })
        .catch(error => console.log(error));
    },
    // Remove Gamer
    removeGamer() {
      const url = `http://localhost:5000/admin/delete-product`;
      console.log(this.RemoveG);
      let data = {
        playerId: this.RemoveG
      };
      console.log(data);
      return axios
        .post(url, data)
        .then(response => {
          console.log(data);
          console.log(response);
          alert("We Removed your Gamer");
        })
        .catch(error => console.log(error));
    }
  }
};
</script>

<style scoped>
.mx-auto {
  background: #09eae2;
  color: azure;
  text-transform: uppercase;
  font-weight: bold;
  text-align: center;
}
</style>
