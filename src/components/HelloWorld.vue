<template>
  <v-container>
    <v-row justify="center">
      <v-spacer />
      <v-col col="4">
        <v-card flat max-width="600">
        <v-carousel height="580">
          <v-carousel-item v-for="myModel in featuredModels" :key="myModel.id">
            <v-card flat height="550">
              <v-card-title class="text-center">{{ myModel.id }}</v-card-title>
              <v-card-text class="text-center">
                <v-img
                  :src="myModel.url"
                  contain
                  max-height="400"
                >
                </v-img>
              </v-card-text>
              <v-card-actions class="text-center">
                {{ myModel.author }}
              </v-card-actions>
            </v-card>
          </v-carousel-item>
        </v-carousel>
        </v-card>
      </v-col>
      <v-spacer />
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  name: "HelloWorld",
  data() {
    return {
      featuredModels: [],
    };
  },
  mounted: async function () {
    this.makeApiCall();
  },
  methods: {
    say: function (msg) {
      alert(msg);
    },
    makeApiCall: async function () {
      console.log("About to make call ...");
      console.log(this.model);
      console.log(this.featuredModels);

      let response;
      try {
        response = await axios.get(
          "https://z4mbar3xtwbqlr4gazjr7w7aga0qgnwn.lambda-url.us-east-1.on.aws/v1/models/featured"
        );
      } catch (error) {
        console.log("axios get failed: ", error);
      }
      if (response) {
        console.log("Server responded with: ", response);
        for (let i = 0; i < response.data.items.length; i++) {
          const myResponse = response.data.items[i];
          this.featuredModels.push({
            id: myResponse.id,
            author: myResponse.author,
            name: myResponse.name,
            url: myResponse.url,
          });
        }
        console.log("featured: ", this.featuredModels);
      }
    },
  },
};
</script>