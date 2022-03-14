<template>
  <div class="repo-wrapper">
    <div class="repo-img">
      <img :src="ownerAvatar" :alt="`Avatar of ${ownerUsername}`">
    </div>
    <div class="repo-details">
      <a :href="repoUrl" target="_blank">
        <h2> {{ name }} </h2>
      </a>
      <p> {{ description }} </p>
      <div class="repo-specifications">
        <div class="badge"> Stars {{ starsCount }} </div>
        <div class="badge"> Issues {{ issuesCount }} </div>
      </div>
      <div> Submitted on {{dateFormatted}} by
        <a :href="ownerUrl" target="_blank">
          {{ ownerUsername }}
        </a>
      </div>
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
    createdAt: String,
    repoUrl: String
  },

  computed: {
    dateFormatted() {
      return moment(this.createdAt).format("DD/MM/YYYY");
    }
  }
}
</script>

<style lang="scss">
.repo-wrapper {
  display: flex;
  flex-wrap: wrap;
  border: 1px solid #d3d3d3;
  border-radius: 6px;
  padding: 15px;
  max-width: 950px;
  margin-bottom: 40px;

  .repo-img {
    width: 250px;
    height: inherit;
    margin-right: 28px;
    border: 1px solid #d3d3d3;
    border-radius: 6px;

    img {
      width: 100%;
      height: 100%;
      border-radius: 4px;
    }
  }
  .repo-details {
    display: flex;
    flex-direction: column;
    width: calc(100% - 280px);

    a {
      text-decoration: none;
      color: #000;
      &:hover {
        text-decoration: underline;
      }
    }

    p {
      overflow: hidden;
      text-overflow: ellipsis;
      display: -webkit-box;
      -webkit-line-clamp: 4;
      -webkit-box-orient: vertical;
    }

    .repo-specifications {
      display: flex;
      margin-bottom: 20px;

      .badge {
        color: #fff;
        background-color: #6c757d;
        display: inline-block;
        padding: 4px 6px;
        font-size: 16px;
        font-weight: 500;
        line-height: 1;
        text-align: center;
        border-radius: 4px;
        margin-right: 8px;
      }
    }
  }
}
</style>
