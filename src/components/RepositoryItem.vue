<template>
  <div class="repo-wrapper">
    <div class="repo-img">
      <img src="https://dummyimage.com/150x150/000/fff.png" alt="repository name">
    </div>
    <div class="repo-details">
      <span> Details </span>
      <span> Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
        Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
        Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RepositoryItem',
  props: {
    repository: {
      name: String,
      description: String,
      starsCount: String,
      issuesCount: String,
      owner: {
        username: String,
        avatar: String
      }
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
}
.repo-img {
  width: 150px;
  height: 150px;
  border: 1px solid;
  margin-right: 30px;
}
.repo-details {
  display: flex;
  flex-direction: column;
}
</style>
