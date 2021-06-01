<template>
  <view class="container">
    <text class="text-color-primary">{{ category.name }}</text>
    <text class="rating">Views: {{ category.popularity }}</text>
    <view v-if="category.articles.length != null" class="article-preview" v-for="article in category.articles.slice(0,2)" v-bindkey="article.id">
      <text class="object">{{ article.object }}</text>
      <text>{{ article.description }}</text>
      <button class="read" v-if="!article.read" title="Select as read" @press="addRating(article.id)" />
      <button class="status" v-if="article.read" title="Marked as read" />
    </view>
    <view class="navigator">
      <button class="nav-button" title="Main" @press="openMain" />
      <button class="nav-button" title="Profile" @press="openProfile" />
    </view>
  </view>
</template>

<script>
export var readCategory = null;
import { category } from './HomeScreen.vue';
export default {
  // Declare `navigation` as a prop
  data() {
    return {
      category: category
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
    addRating(index) {
      this.category.articles[index].read = !this.category.articles[index].read;
      this.category.popularity += 1;
    },
    openMain() {
      readCategory = this.category,
      this.navigation.navigate("Home");
    },
    openProfile() {
      this.navigation.navigate("Profile");
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
.back {
  width: 100%;
  font-size: 14px;
}
.text-color-primary {
  width: 100%;
  color: purple;
  margin-bottom: 20px;
  text-align: center;
  font-size: 30;
}
.rating {
  width: 100%;
  text-align: right;
  font-size: 20px;
}
.article-preview {
  width: 100%;
  margin-bottom: 40px;
  position: relative;
}
.read {
  position: absolute;
  right: 0;
  bottom: -30px;
  font-size: 16px;
  height: 30px;
  width: 180px;
  background-color: yellow;
}
.status {
  position: absolute;
  right: 0;
  bottom: -30px;
  font-size: 16px;
  height: 30px;
  width: 180px;
  background-color: green;
}
.object {
  font-weight: 600;
  font-size: 18px;
}
</style>
