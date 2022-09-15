<template>
  <v-container>
    <v-carousel v-model="model">
      <v-carousel-item v-for="myModel in featuredModels" :key="myModel.id">
        <v-card height="80%">
            <div class="text-h2">{{ myModel.id }}</div>
            <p>
            <v-img :src="myModel.url" height="auto" width="auto"> </v-img>
            </p>
        </v-card>
      </v-carousel-item>
    </v-carousel>
  </v-container>
</template>

<script>
import axios from "axios";
export default {
  name: "HelloWorld",
  data() {
    return {
      model: 0,
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
        response = await axios.get("https://z4mbar3xtwbqlr4gazjr7w7aga0qgnwn.lambda-url.us-east-1.on.aws/v1/models/featured");
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