<template>
  <view class="container">
    <text class="text-color-primary">{{ message }}</text>
    <view class="sub" v-for="category in categories" v-bind:key="category.id" @press="openCategory(category.name)">
      <text>{{ category.name }}</text>
      <text v-if="!category.followed" class="follow" @press="followCategory(category.name)">+</text>
      <text v-if="category.followed" class="unfollow" @press="followCategory(category.name)">X</text>
    </view>
    <view class="navigator">
      <button class="nav-button" title="Main" @press="openMain" />
      <button class="nav-button" title="Profile" @press="openProfile" />
    </view>
  </view>
</template>

<script>
import { subscriptions } from './HomeScreen.vue'
import { db } from './HomeScreen.vue'
export default {
  // Declare `navigation` as a prop
  data() {
    return {
      message: 'Profile',
      categories: subscriptions
    }
  },
  created() {

  },
  props: {
    navigation: {
      type: Object
    }
  },
  methods: {
    openMain() {
      this.navigation.navigate("Home");
    },
    openProfile() {
      this.navigation.navigate("Profile");
    },
    followCategory(name) {
      for (i = 0; i < this.categories.length; i++) {
        if (this.categories[i].name == name) {
          this.categories[i].followed = !this.categories[i].followed;
        }
      }
    }
  }
}
</script>
<style>
.container {
  display: flex;
  flex: 1;
  padding-top: 40px;
  padding-left: 8px;
  padding-right: 8px;
  flex-direction: column;
  flex-wrap: wrap;
  background-color: #c3c3c3;
  justify-content: center;
  position: relative;
}
.navigator {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  width: 100%;
  height: 50px;
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}
.nav-button {
  color: black;
  padding-left: 40px;
  padding-right: 40px;
  height: 100%;
  font-size: 18px;
  text-align: center;
}
.sub {
  width: 100%;
  position: relative: height: 50px;
  margin-bottom: 10px;
  font-size: 14px;
}
.text-color-primary {
  width: 100%;
  color: purple;
  margin-bottom: 20px;
  text-align: center;
  font-size: 30;
}
.follow {
  font-size: 14px;
  font-weight: 800;
  background-color: yellow;
  position: absolute;
  z-index: 10;
  top: 0;
  right: 0;
  width: 20px;
  height: 20px;
  color: black;
  text-align: center;
  line-height: 20px;
}
.unfollow {
  font-size: 14px;
  font-weight: 800;
  background-color: green;
  position: absolute;
  z-index: 10;
  top: 0;
  right: 0;
  width: 20px;
  height: 20px;
  color: white;
  text-align: center;
  line-height: 20px;
}
</style>
