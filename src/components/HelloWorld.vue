<template>
  <div class="gw-profile">
    <p v-if="errMsg" class="gw-errorMsg" >{{ errMsg }}</p>
    <template v-else>
      <div class="gw-profile__avatar">
        <div class="gw-profile__avatarl">
          <img :src="profile.avatar_url" alt="avatar">
          <a class="gw-profile__avatarl--follow" :href="profile.html_url" target="_blank">
            Follow
          </a>
        </div>
        <div class="gw-profile__avatarr">
          <h3 class="gw-profile__avatarr--name">{{ profile.name }}</h3>
          <p class="gw-profile__avatarr--bio">{{ profile.bio }}</p>
          <p class="gw-profile__avatarr--loc">
            ⚲ {{ profile.location }}
          </p>
        </div>
      </div>
      <div class="gw-profile__desc">
        <div class="gw-profile__desc--item">
          <span>{{ profile.followers }}</span>
          <p>Followers</p>
        </div>
        <div class="gw-profile__desc--item">
          <span>{{ profile.following }}</span>
          <p>Following</p>
        </div>
        <div class="gw-profile__desc--item">
          <span>{{ profile.public_repos }}</span>
          <p>Repositories</p>
        </div>
      </div>
      <div class="gw-profile__repos">
        <h4>Top 3 repositories</h4>
        <ul>
          <li v-for="repo in repos.slice(0, 3)" :key="repo.id">
            <span class="gw-profile__repos--name">
              <a :href="repo.html_url" target="_blank">
                {{ repo.name.length > 20 ? repo.name.slice(0, 20) + '...' : repo.name }}
              </a>
            </span>
            <span class="gw-profile__repos--lan">{{ repo.language }}</span>
            <span class="gw-profile__repos--count">★{{ repo.stargazers_count }}</span>
          </li>
        </ul>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  name: 'GithubWidget',
  data() {
    return {
      profile: {},
      repos: [],
      errMsg: '',
    };
  },
  methods: {
    getUserProfile() {
      return this.$http('https://api.github.com/users/HuangXiZhou');
    },
    getUserRepos() {
      return this.$http('https://api.github.com/users/HuangXiZhou/repos');
    },
    quickSort(arr) {
      if (arr.length <= 1) { return arr; }
      const pivotIndex = Math.floor(arr.length / 2);
      const pivot = arr.splice(pivotIndex, 1)[0];
      const left = [];
      const right = [];

      for (let i = 0; i < arr.length; i++) {
        if (arr[i].stargazers_count > pivot.stargazers_count) {
          left.push(arr[i]);
        } else {
          right.push(arr[i]);
        }
      }

      return this.quickSort(left).concat([pivot], this.quickSort(right));
    },
  },
  mounted() {
    this.$http.all([this.getUserProfile(), this.getUserRepos()])
      .then(this.$http.spread((profile, repos) => {
        this.profile = profile.data;
        this.repos = this.quickSort(repos.data);
      }))
      .catch((err) => {
        this.errMsg = err.message;
      });
  },
};
</script>

<style lang="less" scoped>
.gw-profile {
  padding: 5px;
  &__avatar {
    display: flex;
    justify-content: center;
    &l {
      flex: 1;
      text-align: center;
      align-self: center;
      img {
        margin: 10px 0 13px 0;
        width: 5em;
        height: 5em;
        border: 1px solid #ddd;
        border-radius: 50%;
      }
      &--follow {
        padding: .1em 1em;
        height: 10px;
        background: #ddd;
        border-radius: 3px;
        color: #333;
        text-decoration: none;
        transition: all .2s ease-in-out;
        &:hover {
          background: #aaa;
          color: #fff;
        }
      }
    }
    &r {
      padding: 0 0 0 5px;
      flex: 2;
      text-align: left;
      &--name {
        margin: 10px 0 5px 0;
        font-size: 30px;
      }
      &--bio {
        margin: 5px 0 5px 0;
        font-size: 14px;
        color: #6a737d;
      }
      &--loc {
        margin: 8px 0 5px 0;
        font-size: 12px;
        color: #24292e;
      }
    }
  }
  &__desc {
    margin-top: 20px;
    display: flex;
    justify-content: center;
    border-bottom: 2px dotted #efefef;
    &--item {
      flex: 1;
      text-align: center;
      & > span {
        font-size: 24px;
        font-weight: 600;
        color: #0366d6;
      }
      & > p {
        margin: 1em 0 1em 0;
      }
    }
  }
  &__repos {
    & > h4 {
      margin: 1em 0 .5em 0;
      text-align: center;
    }
    & > ul {
      margin: 0;
      padding: 0;
      list-style: none;
      & > li {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 35px;
        // background: #333;
        & > span {
          text-align: left;
        }
        .gw-profile__repos--name {
          padding: 0 0 0 5px;
          flex: 3;
          & > a {
            text-decoration: none;
            color: #0366d6;
            &:hover {
              text-decoration: underline;
            }
          }
        }
        .gw-profile__repos--lan {
          flex: 2;
          color: #777;
        }
        .gw-profile__repos--count {
          flex: 1;
          color: #675741;
        }
      }
    }
  }
}
</style>
