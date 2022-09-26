<template>
  <v-app id="inspire">
    <v-container style="height: 100%" class="grey lighten-5" >
    <div style = "margin-top: 100px;">
    </div>
    <v-row style="height: 14rem">
       <v-col cols="12" sm="4" v-for ="L1 in selectedLinks" :key="L1.ID">
      <span v-if = "!Array.isArray(L1)">
        <component v-bind:is="L1.componentType" />
        {{L1.message}}
      </span>
        <span v-else>
          <v-row style="height: 7rem" v-for ="L2 in L1" :key="L2.ID">
          <v-col>
            <span v-if = "!Array.isArray(L2)">
              <component v-bind:is="L2.componentType" />
              {{L2.message}}
            </span>
            <span v-else>
              <v-row style="height: 7rem">
                <v-col v-for ="L3 in L2" :key="L3.ID">
                  <span v-if = "!Array.isArray(L3)">
                  <component v-bind:is="L3.componentType" />
                  {{L3.message}}
                  </span>
                </v-col>
              </v-row>
            </span>
          </v-col>
          </v-row>
        </span>
      </v-col>
    </v-row>
    
    </v-container>
  </v-app>
</template>

<script>
export default {
    name: 'app',
    props: {selectedLinks: Array},
    components: {
    },
    data (){
      return {
      }
    },
};
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px; 
  height: 100%;
  flex-grow: 1;
  display: flex;
  flex-direction: column;

}
html, body, #app {
  height: 100%;
  margin: 0;
}

.small {
  background-color: white;
  border: 1px solid black; 
  height: 6rem;
  width: 10rem;
}

.medium {
  background-color: white;
  border: 1px solid black;
  height: 6rem;
  width: 22.3rem;
  
}

.large {
  background-color: white;
  border: 1px solid black;
  height: 13rem;
  width: 22.3rem;
}

</style>
