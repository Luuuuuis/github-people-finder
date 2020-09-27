<template>
  <div id="app" class="container">
    <div id="top">
      <div id="headline">
        <h1>Crazy github people finder</h1>
      </div>
      <div id="content">
        <div id="input">
          <form @submit.prevent="" class="form">
            <div class="row justify-content-center">
              <div class="col-auto">
                <label for="user-input" class="sr-only">User</label>
                <input
                  v-model.lazy="user"
                  id="user-input"
                  placeholder="Username"
                  class="form-control"
                />
              </div>
              <div class="col-auto">
                <button type="submit" class="btn btn-primary">Search</button>
              </div>
            </div>
          </form>
        </div>
      </div>

      <div v-if="user != null">
        <div id="user" class="padding">
          <User :username="user" @loading="user_loading = $event"></User>
        </div>

        <div id="repos" class="padding">
          <Repos
            :username="user"
            @loading="repos_loading = $event"
            class="d-flex justify-content-center"
          ></Repos>
        </div>

        <div v-show="user_loading == true || repos_loading == true">
          <Spinner></Spinner>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Repos from "./components/Repos";
import User from "./components/User";
import Spinner from "./components/Spinner";

export default {
  name: "App",
  data() {
    return {
      user: "Luuuuuis",
      user_loading: false,
      repos_loading: false,
    };
  },
  methods: {},
  components: {
    Repos,
    User,
    Spinner,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

#top {
  text-align: center;
}

.padding {
  padding-top: 20px;
}
</style>
