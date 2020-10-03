<template>
  <div v-if="info != null">
    <div class="d-flex">
      <img :src="info.avatar_url" width="250" height="250" />
      <div class="row justify-content-start" style="width: 50%">
        <div id="info">
          <div class="col">
            <h2>
              {{ info.login }}
              <span v-if="info.name != null">({{ info.name }})</span>
            </h2>
          </div>
          <div id="facts">
            <div class="col">
              <h6>
                <span v-if="info.type == 'User'">
                  <i class="fas fa-users"></i> {{ info.followers }} followers ●
                  {{ info.following }} following ●
                </span>
                <span v-if="info.location != null">
                  <i class="fas fa-map-marked-alt"></i>
                  {{ info.location }} ● </span
                ><i class="far fa-calendar-alt"></i> {{ timeFormatted }}
              </h6>
            </div>
          </div>
          <div id="bio">
            <div class="col">
              <h5>{{ info.bio }}</h5>
            </div>
          </div>
        </div>
        <div id="time" class="mt-auto"></div>
      </div>
      <div class="ml-auto" id="social">
        <a :href="info.html_url" id="social_github"
          ><i class="fab fa-github"></i
        ></a>
        <a v-if="info.blog != null" :href="info.blog" id="social_blog"
          ><i class="fas fa-link"></i
        ></a>
        <a
          v-if="info.twitter_username != null"
          :href="'https://twitter.com/' + info.twitter_username"
          id="social_twitter"
          ><i class="fab fa-twitter"></i
        ></a>
      </div>
    </div>
  </div>
  <div v-else-if="err != null">
    <div v-if="err.status == 404">
      <h1>This user doesn't exists :''(</h1>
    </div>
    <div v-else>
      <p>Error: {{ err.status + " " + err.data.message }}</p>
    </div>
  </div>
</template>

<script>
const axios = require("axios").default;
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
        .get("https://api.github.com/users/" + this.username)
        .then((response) => {
          this.info = response.data;
        })
        .catch((error) => {
          this.err = error.response;
        });
      this.$emit("loading", false);
    },
  },
  computed: {
    timeFormatted: function() {
      let date = new Date(Date.parse(this.info.created_at));

      return date.toLocaleString();
    },
  },
};
</script>

<style scoped>
img {
  border-radius: 5%;
  -webkit-box-shadow: 20px 14px 38px -5px rgba(66, 63, 66, 0.5);
  -moz-box-shadow: 20px 14px 38px -5px rgba(66, 63, 66, 0.5);
  box-shadow: 20px 14px 38px -5px rgba(66, 63, 66, 0.5);
}

#info {
  padding-left: 2rem;
  padding-top: 5px;
  text-align: left;
  width: 100%;
}

#facts,
#bio {
  padding-left: 0.2rem;
}

#bio {
  padding-top: 1rem;
}

#visit_button:hover {
  -webkit-box-shadow: 2px 2px 20px 4px rgba(66, 63, 66, 0.5);
  -moz-box-shadow: 2px 2px 20px 4px rgba(66, 63, 66, 0.5);
  box-shadow: 2px 2px 20px 4px rgba(66, 63, 66, 0.5);
}

#social {
  padding-right: 2rem;
  padding-top: 5px;
  text-align: right;
  font-size: 2em;
  text-decoration: none;
}

#social_twitter,
#social_blog,
#social_github {
  color: black;
  transition: 0.5s;
}

#social_blog,
#social_github {
  padding-right: 1rem;
}

#social_twitter:hover,
#social_blog:hover,
#social_github:hover {
  color: dodgerblue;
}
</style>
