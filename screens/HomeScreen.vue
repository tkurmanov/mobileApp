<template>
  <view class="container">
    <text class="text-color-primary">{{ message }}</text>
    <view class="fandom" v-if="sorting == 1" v-for="category in latestCategories.slice(0, 4)" v-bind:key="category.id" @press="openCategory(category.name)">
      <view class="paddinger">
        <view class="holder">
          <view class="body">
            <image class="cover" :source="{uri: category.img}"/>
            <text v-if="!category.followed" class="follow" @press="followCategory(category.name)">+</text>
            <text v-if="category.followed" class="unfollow" @press="followCategory(category.name)">X</text>
            <text class="title" @press="openCategory(category.name)"> {{ category.name }}</text>
          </view>
        </view>
      </view>
    </view>
    <view class="fandom" v-if="sorting == 2" v-for="category in popularCategories.slice(0, 4)" @press="openCategory(category.name)" v-bind:key="category.id">
      <view class="paddinger">
        <view class="holder">
          <view class="body">
            <image class="cover" :source="{uri: category.img}"/>
            <text v-if="!category.followed" class="follow" @press="followCategory(category.name)">+</text>
            <text v-if="category.followed" class="unfollow" @press="followCategory(category.name)">X</text>
            <text class="title" @press="openCategory(category.name)"> {{ category.name }}</text>
          </view>
        </view>
      </view>
    </view>
    <view class="fandom-all" v-if="sorting == 3" v-for="category in latestCategories" v-bind:key="category.id" @press="openCategory(category.name)">
      <view class="paddinger">
        <view class="holder">
          <view class="body">
            <image class="cover" :source="{uri: category.img}"/>
            <text v-if="!category.followed" class="follow" @press="followCategory(category.name)">+</text>
            <text v-if="category.followed" class="unfollow" @press="followCategory(category.name)">X</text>
            <text class="title-all" @press="openCategory(category.name)"> {{ category.name }}</text>
          </view>
        </view>
      </view>
    </view>
    <button v-if="sorting != 1" title="Show Latest" @press="showLatest" />
    <button v-if="sorting != 2" title="Show Popular" @press="showMostPopular" />
    <button v-if="sorting != 3" title="Show All" @press="showAll" />
    <view class="navigator">
      <button class="nav-button" title="Main" @press="openMain" />
      <button class="nav-button" title="Profile" @press="openProfile" />
    </view>
  </view>
</template>

<script>
import axios from 'axios';
import Stringify from 'vue-stringify';
import { readCategory } from './DetailsScreen.vue';
export var path = {
  state: {
    destination: null,
    index: 0,
  }
}
export var subscriptions = [];
export var category = null;
export var ratings = [26, 14, 30, 18, 19, 13, 12, 20, 14];
export var db = [
  {
    id: 0,
    name: 'Music',
    img: 'http://4.bp.blogspot.com/-F9JWRfxywwk/UvZSbwAt78I/AAAAAAAADuI/7Pex6zrXfbE/s1600/Clave-de-sol-Music-iPhone-4-wallpaper.jpg',
    followed: false,
    popularity: ratings[0]
  },
  {
    id: 1,
    name: 'Science',
    img: 'https://wallpapercave.com/wp/wp5334649.jpg',
    followed: false,
    popularity: ratings[1]
  },
  {
    id: 2,
    name: 'Anime',
    img: 'https://wallpapercave.com/wp/wp5883545.jpg',
    followed: false,
    popularity: ratings[2]
  },
  {
    id: 3,
    name: 'Marvel',
    img: 'https://www.nawpic.com/media/2020/marvel-phone-nawpic-1.jpg',
    followed: false,
    popularity: ratings[3]
  },
  {
    id: 4,
    name: 'DC',
    img: 'https://wallpapercave.com/wp/wp5386637.jpg',
    followed: false,
    popularity: ratings[4]
  },
  {
    id: 5,
    name: 'JavaScript',
    img: 'https://cdn.hipwallpaper.com/i/45/92/21rOxq.jpg',
    followed: false,
    popularity: ratings[5],
    articles: [
      {
        id: 0,
        object: 'VueJS',
        description: 'Vue. js (pronounced /vjuː/, like view) is a library for building interactive web interfaces. The goal of Vue. js is to provide the benefits of reactive data binding and composable view components with an API that is as simple as possible.',
        read: false
      },
      {
        id: 1,
        object: 'Angular',
        description: 'Angular (commonly referred to as "Angular 2+" or "Angular v2 and above") is a TypeScript-based open-source web application framework led by the Angular Team at Google and by a community of individuals and corporations. Angular is a complete rewrite from the same team that built AngularJS. Angular is used as the frontend of the MEAN stack, consisting of MongoDB database, Express.js web application server framework, Angular itself (or AngularJS), and Node.js server runtime. environment.',
        read: false
      },
      {
        id: 2,
        object: 'ReactJS',
        description: 'React (also known as React.js or ReactJS) is an open-source front-end JavaScript library for building user interfaces or UI components. It is maintained by Facebook and a community of individual developers and companies.[4][5][6] React can be used as a base in the development of single-page or mobile applications. However, React is only concerned with state management and rendering that state to the DOM, so creating React applications usually requires the use of additional libraries for routing, as well as certain client-side functionality.',
        read: false
      }
    ]
  },
  {
    id: 6,
    name: 'Geography',
    img: 'https://i.pinimg.com/originals/40/b2/f6/40b2f69f6014dd51d1c9293308b0a700.jpg',
    followed: false,
    popularity: ratings[6]
  },
  {
    id: 7,
    name: 'Weapon',
    img: 'https://i.pinimg.com/originals/10/c9/3d/10c93d1892e163e08c1640fb670f1f29.jpg',
    followed: false,
    popularity: ratings[7]
  },
  {
    id: 8,
    name: 'Plants',
    img: 'https://i.pinimg.com/originals/3d/27/4c/3d274c73ef7122ae6cf80631f6d2a694.jpg',
    followed: false,
    popularity: ratings[8]
  }
]

export default {
  data() {
    return {
      message: "Main Page",
      sorting: 1,
      all: false,
      info: [],
      categories: db
    }
  },
  props: {
    navigation: {
      type: Object
    }
  },
  computed: {
    latestCategories: function() {
      return this.categories.reverse();
    },
    popularCategories: function() {
      var _ = require('lodash');
      return _.orderBy(this.categories, 'popularity').reverse();
    }
  },
  created() {
    let app = this;
    axios
      .get('http://www.randomnumberapi.com/api/v1.0/random?min=5&max=50&count=9')
      .then(response => {
        this.info = response.data;
        this.info.map(function(value, key){
          app.categories[key].popularity = value;
        });
      });
      if (subscriptions.length != 0) {
        this.categories = subscriptions;
      }
      if (readCategory != null && realCategory.length != 0) {
        this.categories[path.state.index] = readCategory;
      }
      // alert(this.info);

  },
  methods: {
    openCategory(name) {
      path.state.destination = name;
      for (i = 0; i < db.length; i++) {
        if (path.state.destination == db[i].name) {
          path.state.index = i;
        }
      }
      category = this.categories[path.state.index];
      this.navigation.navigate("Details");
    },
    followCategory(name) {
      for (i = 0; i < this.categories.length; i++) {
        if (this.categories[i].name == name) {
          this.categories[i].followed = !this.categories[i].followed;
        }
      }
    },
    showLatest() {
      this.sorting = 1;
    },
    showMostPopular() {
      this.sorting = 2;
    },
    showAll() {
      this.sorting = 3;
    },
    openMain() {
      this.navigation.navigate("Home");
    },
    openProfile() {
      subscriptions = this.categories;
      this.navigation.navigate("Profile");
    }
  },
};
</script>

<style>
.container {
  display: flex;
  flex: 1;
  padding-top: 40px;
  padding-left: 8px;
  padding-right: 8px;
  flex-direction: row;
  flex-wrap: wrap;
  background-color: #c3c3c3;
  align-items: center;
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
.row {
  display: flex;
  flex-direction: row;
  flex-wrap: nowrap;
}
.fandom {
  width: 50%;
  padding: 10px;
}
.fandom-all {
  width: 32%;
  padding: 0.6666%;
}
.paddinger {
  width: 100%;
  padding-top: 132%;
  position: relative;
}
.holder {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  z-index: 7;
}
.body {
  position: relative;
  width: 100%;
  height: 100%;
  z-index: 1;
  border-radius: 8px;
}
.cover {
  width: 100%;
  height: 100%;
  position: relative;
  z-index: 5;
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
.title {
  color: green;
  font-weight: 600;
  font-size: 20px;
  text-align: center;
  text-transform: uppercase;
  position: absolute;
  z-index: 5;
  left: 0;
  right: 0;
  bottom: 0;
  top: auto;
  background-color:rgba(0,0,0,0.6);
  padding: 10px;
}
.title-all {
  color: green;
  font-weight: 600;
  font-size: 12px;
  text-align: center;
  text-transform: uppercase;
  position: absolute;
  z-index: 5;
  left: 0;
  right: 0;
  bottom: 0;
  top: auto;
  background-color:rgba(0,0,0,0.6);
  padding: 10px;
}
.text-color-primary {
  width: 100%;
  color: purple;
  margin-bottom: 20px;
  text-align: center;
  font-size: 30;
}
</style>
