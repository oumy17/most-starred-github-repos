<template>
  <div id="app">
    <div class="layout">
      <div class="date-filter">
        <Datepicker v-model="selectedDate" @selected="onSelect"></Datepicker>
      </div>
      <RepositoryItem v-for="(repo, index) in repos" :key="index"
        :name="repo.name"
        :description="repo.description"
        :starsCount="repo.stargazers_count"
        :issuesCount="repo.open_issues_count"
        :ownerAvatar="repo.owner.avatar_url"
        :ownerUsername="repo.owner.login"
        :ownerUrl="repo.owner.html_url"
        :createdAt="repo.created_at">
      </RepositoryItem>
      <infinite-loading :identifier="infiniteId" @infinite="infiniteHandler">
        <div slot="no-more">No more repositories</div>
        <div slot="no-results">No results</div>
      </infinite-loading>
    </div>
  </div>
</template>

<script>
import RepositoryItem from './components/RepositoryItem.vue'
import InfiniteLoading from 'vue-infinite-loading';
import moment from "moment";
import Datepicker from 'vuejs-datepicker';
import axios from "axios";

export default {
  name: 'App',
  components: {
    RepositoryItem,
    Datepicker,
    InfiniteLoading
  },

  data() {
    return {
      repos: [],
      page: 1,
      selectedDate: this.customFormatter(new Date()),
      infiniteId: +new Date(),
    }
  },

  methods: {

    customFormatter(date) {
      return moment(date).format("YYYY-MM-DD");
    },

    onSelect() {
      this.page = 1;
      this.repos = [];
      this.infiniteId += 1;
    },

    infiniteHandler($state) {
      const formattedDate = this.customFormatter(this.selectedDate)
      const apiEndpoint = `https://api.github.com/search/repositories?q=created:>${formattedDate}&sort=stars&order=desc`;
      axios.get(apiEndpoint, {
        params: {
          page: this.page,
        },
      }).then(({ data }) => {
        if (data.items.length) {
          this.page += 1;
          this.repos.push(...data.items);
          $state.loaded();
        } else {
          this.page = 1
          $state.complete();
        }
      })
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 900px;
  margin: 0 auto;
}
.layout {
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin-top: 60px;
}
.date-filter {
  align-self: flex-end;
  margin-bottom: 30px;
}
</style>
