<template>
  <nav>
    <v-app-bar app flat class="white">
         <v-app-bar-nav-icon class = "black--text" @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title class = "text-upercase black--text">
        <span class = "font-weight-light">
        Call Simulator
        </span>
      </v-toolbar-title> 
      </v-app-bar>
  <v-navigation-drawer app v-model = "drawer" class = 'blue'>
    <v-list>
        <v-list-item v-for = "link in links" :key="link.message">
            <v-list-item-content>
                <v-list-item-title class ="white--text" v-text = "link.message" @click = "$emit('addingToList', link)">
                </v-list-item-title>
            </v-list-item-content>
        </v-list-item>
    </v-list>
    <v-btn @click="$emit('removingFromList')">
      undo
  </v-btn>
  <br><br/>
    <v-btn @click="drawer = false">
      close
  </v-btn>
  </v-navigation-drawer>  
  </nav>
</template>

<script>
export default {
  data() {
    return {
      drawer: false,
      links: [
          {message: 'Card 1 (large)', size: "large", ID: 1, componentType: "Card1"} ,
          {message: 'Card 2 (medium)', size: "medium", ID: 2, componentType: "Card2"},
          {message: 'Card 3 (small)', size: "small", ID: 3, componentType: "Card3"},
          {message: 'Card 4 (medium)', size: "medium", ID: 4, componentType: "Card4"},
          {message: 'Card 5 (large)', size: "large", ID: 5, componentType: "Card5"},
          {message: 'Card 6 (medium)', size: "medium", ID: 6, componentType: "Card6"},
          {message: 'Card 7 (small)', size: "small", ID: 7, componentType: "Card7"},
          {message: 'Card 8 (medium)', size: "medium", ID: 8, componentType: "Card8"},
          {message: 'Card 9 (small)', size: "small", ID: 9, componentType: "Card9"},
          {message: 'Card 10 (small)', size: "small", ID: 10, componentType: "Card10"},
      ],
      selectedLinks: [
          
      ]
    }
  },
};
</script>

<style>
v-navigation-drawer {
    padding: 10;
    display: flex;
    justify-content: space-between;
    list-style-type: none;
}
</style>