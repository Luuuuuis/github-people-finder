<template>
  <div v-if="info != null" class="card-deck">
    <div v-for="inf in info" :key="inf.id">
      <div class="card">
        <div class="card-body d-flex flex-column">
          <h5 class="card-title">{{ inf.name }}</h5>
          <p class="card-text">
            {{ getDescription(inf.description) }}
          </p>
          <div class="mt-auto">
            <a :href="inf.html_url" class="card-link">GitHub</a>
            <a :href="inf.html_url.concat('/releases')" class="card-link"
              >Download</a
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const axios = require("axios");

export default {
  props: {
    username: String,
  },
  data() {
    return {
      info: this.getData(),
      err: null,
    };
  },
  watch: {
    username: function() {
      this.getData();
    },
  },
  methods: {
    getData() {
      this.$emit("loading", true);
      this.err = null;
      this.info = null;

      axios
        .get("https://api.github.com/users/" + this.username + "/repos")
        .then((response) => {
          this.info = response.data;
        })
        .catch((error) => {
          this.err = error.response;
        });
      this.$emit("loading", false);
    },
    getDescription(desc) {
      if (desc.length > 87) {
        return desc.substring(0, 84).concat("...");
      } else {
        return desc;
      }
    },
  },
};
</script>

<style scoped>
.card {
  margin: 1rem;
  width: 20rem;
  height: 12rem;
}
</style>
