<template>
  <div class="repo-wrapper">
    <div class="repo-img">
      <img :src="ownerAvatar" :alt="`Avatar of ${ownerUsername}`">
    </div>
    <div class="repo-details">
      <h2> {{ name }} </h2>
      <p> {{ description }} </p>
      <div> Stars {{ starsCount }} </div>
      <div> Issues {{ issuesCount }} </div>
      <div> Submitted on {{dateFormatted}} by {{ ownerUsername }} </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";

export default {
  name: 'RepositoryItem',
  props: {
    name: String,
    description: String,
    starsCount: Number,
    issuesCount: Number,
    ownerUsername: String,
    ownerAvatar: String,
    ownerUrl: String,
    createdAt: String
  },

  computed: {
    dateFormatted() {
      return moment(this.createdAt).format("DD/MM/YYYY");
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.repo-wrapper {
  border: 1px solid lightgray;
  border-radius: 4px;
  padding: 15px;
  display: flex;
  max-width: 950px;
  margin-bottom: 40px;
}
.repo-img {
  width: 150px;
  height: 150px;
  margin-right: 30px;
  border: 1px solid lightgray;
  border-radius: 4px;
}
img {
  width: 100%;
  height: 100%;
}
.repo-details {
  display: flex;
  flex-direction: column;
}
</style>
