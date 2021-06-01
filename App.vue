<template>
  <app-navigator></app-navigator>
</template>

<script>
  import {
    createAppContainer,
    createStackNavigator,
  } from "vue-native-router";


  import HomeScreen from "./screens/HomeScreen.vue";
  import DetailsScreen from "./screens/DetailsScreen.vue";
  import ProfileScreen from "./screens/ProfileScreen.vue";



  const StackNavigator = createStackNavigator(
    {
      Home: HomeScreen,
      Details: DetailsScreen,
      Profile: ProfileScreen
    },
    {
      initialRouteName: 'Home',
      headerMode: 'none',
      swipeEnabled: 'false'
    }
  );

  const AppNavigator = createAppContainer(StackNavigator);

  export default {
    components: { AppNavigator }
  }
</script>
