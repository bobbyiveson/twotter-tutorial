<template>
<div class="user-profile" v-cloak>
  <div class="user-profile__user-panel">
    <h1 class="user-profile__username">@{{ user.username }}</h1>
    <div class="user-profile__admin-badge" v-if="user.isAdmin">Admin</div>
    <div class="user-profile__follower-count">
      <strong>Followers: </strong> {{ followers }}
    </div>
    <form class="user-profile__create-twoot" @submit.prevent="createNewTwoot" :class="{ '--exceeded': newTwootCharacterCount > 180 }">
      <label for="newTwoot"><strong>New Twoot:</strong> ({{ newTwootCharacterCount }}/180)</label>
      <textarea id="newTwoot" rows="4" v-model="newTwootContent" />
      <div class="user-profile__create-twoot-type">
        <label for="newTwootType"><strong>Type:</strong></label>
        <select id="newTwootType" v-model="selectedTwootType">
          <option v-for="(option, index) in twootTypes" :key="index" :value="option.value">
            {{ option.name }}
          </option>
        </select>
      </div>
      <button>Twoot!</button>
    </form>
  </div>
  <div class="user-profile__twoots-wrapper">
    <TwootItem
      v-for="twoot in user.twoots"
      :key="twoot.id"
      :username="user.username"
      :twoot="twoot"
      @favourite="toggleFavourite"
    />
  </div>
</div>
</template>

<script>
import TwootItem from '@/components/TwootItem.vue'
let id = 0
export default {
  name: 'UserProfile',
  data() {
    return {
      newTwootContent: '',
      selectedTwootType: 'instant',
      followers: 0,
      twootTypes: [
        { value: 'draft', name: 'Draft'},
        { value: 'instant', name: 'Instant Twoot'}
      ],
      user: {
        id: 1,
        username: '_bobbyiveson',
        firstName: 'Bobby',
        lastName: 'Iveson',
        email: 'bobby.j.iveson@gmail.com',
        isAdmin: true,
        twoots: [
          { id: ++id, content: 'Twooter is Amazing' },
          { id: ++id, content: "Don't forget to subscribe to The World is Square" }
        ]
      },
    }
  },
  watch: {
    followers(newFollowersCount, oldFollowersCount) {
      if(newFollowersCount > oldFollowersCount) {
        console.log(`${this.user.username} gained a new follower!`)
      }
    }
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`
    },
    newTwootCharacterCount() {
      return this.newTwootContent.length;
    }
  },
  methods: {
    followUser() {
      this.followers++ 
    },
    toggleFavourite(id) {
      console.log(`Favourited Twoot #${id}`)
    },
    createNewTwoot() {
      if(this.newTwootContent && this.selectedTwootType != 'draft') {
        this.user.twoots.unshift({ id: ++id, content: this.newTwootContent })
        this.newTwootContent = "";
      }
    },
  },
  mounted() {
    this.followUser()
  },
  components: {
    TwootItem
  }
}
</script>

<style lang="scss" scoped>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  padding: 50px 5%;
  .user-profile__user-panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #DFE3E8;
    h1 {
      margin: 10px;
    }
    .user-profile__create-twoot {
      /* border-top: 1px solid #DFE3E8; */
      padding-top: 20px;
      display: flex;
      flex-direction: column;
      &.--exceeded {
        #newTwoot {
          color: red;
          border: red;
        }
        button {
          color: white;
          background-color: red;
        }
      }
    }
    .user-profile__admin-badge {
      background-color: rebeccapurple;
      color: white;
      border-radius: 5px;
      margin-right: auto;
      padding: 0 10px;
      font-weight: bold;
    }
  }
}
[v-cloak] {
  display: none;
}
</style>