<template>
  <div id="app">
    <div class="layout">
      <h1> Get the most starred github repositories from the date you select until 30 days after</h1>
      <div class="date-filter">
        <Datepicker v-model="selectedDate" input-class="form-control" calendar-class="jo" @selected="onSelect"></Datepicker>
      </div>
      <RepositoryItem v-for="(repo, index) in repos" :key="index"
        :name="repo.name"
        :description="repo.description"
        :starsCount="repo.stargazers_count"
        :issuesCount="repo.open_issues_count"
        :ownerAvatar="repo.owner.avatar_url"
        :ownerUsername="repo.owner.login"
        :ownerUrl="repo.owner.html_url"
        :createdAt="repo.created_at"
        :repoUrl="repo.html_url">
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
      const fromDate = moment(this.selectedDate).format("YYYY-MM-DD")
      const toDate = moment(this.selectedDate).add(30, "days").format("YYYY-MM-DD")
      const apiEndpoint = `https://api.github.com/search/repositories?q=created:${fromDate}..${toDate}&sort=stars&order=desc`;
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
      .catch((error) => {
        alert(`${error.response.data.message}, please refresh the page or select another date`)
      });
    },
  }
}
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css?family=Lato:400,700,900");
@import url('https://fonts.googleapis.com/css2?family=Montserrat');

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  max-width: 900px;
  margin: 0 auto;
  font-family: 'Montserrat', sans-serif;
  .layout {
    display: flex;
    flex-direction: column;
    justify-content: center;
    margin-top: 60px;

    .date-filter {
      align-self: flex-end;
      margin-bottom: 30px;

      .vdp-datepicker {

        input[type=text] {
          padding: 9px;
          margin: 8px 0;
        }
      }
    }
  }
}

</style>
