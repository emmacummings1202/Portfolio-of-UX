<template>
  <v-app>
    <navBar @addingToList = "recieveData" 
            @removingFromList = "undoBtn"/>
    <cards :selectedLinks = "selectedLinks" />
  </v-app>
</template>

<script>
import navBar from "@/components/NavBar.vue";
import cards from "@/components/Cards.vue";
export default {
  name: "App",
  components: { navBar, cards },
  data() {
    return {
      selectedLinks: [],
      currLayer: 1,
      layer1Index: -1,
      layer2Index: -1,
      layer3Index: -1,
      //bool to see if you can put a small next
      ss: false,
      smallComplete: false,
      medComplete: false,
      exists: false,
      };
  },
  methods: {
    undoBtn: function() {
      let length = this.selectedLinks.length;
      //if the last element of the layer 1 array is an array
      if(Array.isArray(this.selectedLinks[length - 1])) {
        //if the last element of the layer 2 array is an array
        if(Array.isArray(this.selectedLinks[length - 1][this.selectedLinks[length - 1].length - 1]) ) {
          //if there are 2 elements in the array
          if (this.selectedLinks[length - 1].length == 2) {
            this.selectedLinks[length - 1][1].pop(); //this is only the case where there are 3 or 4 smalls 
            //if, after removing the element, the array still has elements
            if (this.selectedLinks[length - 1][1].length > 0) {
              //the current layer is still 3
              this.currLayer = 3;
              this.ss = true;
              this.layer3Index = 0;
              this.layer2Index = 1;
            } else {
              //if there are no more elements, the current layer is 2
              this.selectedLinks[length - 1].pop();
              this.currLayer = 2;
              this.layer2Index = 0;
              this.ss = false;
              this.layer3Index = -1;
            }
          } else {
            this.selectedLinks[length - 1][0].pop(); //when there are 1 or 2 smalls 
            //if, after removing the element, the array still has elements
            if (this.selectedLinks[length - 1][0].length > 0) {
              //the current layer is still 3
              this.currLayer = 3;
              this.ss = true;
              this.layer3Index = 0;
              this.layer2Index = 0;
            } else {
              //if there are no more elements, the current layer is 2
              this.selectedLinks.pop();
              this.layer2Index = -1;
              this.currLayer = 1;
              this.ss = false;
              this.layer3Index = -1;
              if (this.selectedLinks.length < 1) {
                this.layer1Index = -1;
              } else {
                this.layer1Index--;
              }
            }
          }
          //removing an element in the 2nd layer
        } else {
          this.selectedLinks[length - 1].pop();
          //if, after removing the element, the array still has elements
          console.log(this.selectedLinks[length - 1]);
          if (this.selectedLinks[length - 1] == 0) {
             //if there are no more elements, the current layer is 1
            this.currLayer = 1;
            this.selectedLinks.pop();
            this.layer1Index--;
            this.layer2Index = -1;
            this.medComplete = true;
          } else {
            //the current layer is 2
            this.currLayer = 2;
            this.layer2Index = 0;
            this.medComplete = false;
          }
         
        }
        //staying on layer 1 and removing
      } else {
        this.selectedLinks.pop();
        this.layer1Index = this.selectedLinks.length - 1;
        this.currLayer = 1;
      }

      //bool to see if you can put a small next
      this.smallComplete = false;

    },

    //function to see if the small layer is complete and L1 index can be increased
    sComplete: function(layer2Index, layer3Index) {
      if (layer2Index === 1 && layer3Index === 1) {
        this.smallComplete = true;
      } else {
        this.smallComplete = false;
      }  
    },
    
    //function to see if the medium layer is complete and L1 index can be increased
    mComplete: function(layer2Index) {
      if (layer2Index === 1) {
        this.medComplete = true;
      } else {
        this.medComplete = false;
      } 
    },

    linkExists: function(link) {
      this.exists = false;
      for (let i = 0; i < this.selectedLinks.length; i++) {
        if (this.selectedLinks[i].ID == link.ID) {
          this.exists = true;
          return;
        }
        for (let j = 0; j < this.selectedLinks[i].length; j++) {
          if (this.selectedLinks[i][j].ID == link.ID) {
            this.exists = true;
            return;
          }
          for (let k = 0; k < this.selectedLinks[i][j].length; k++) {
          if (this.selectedLinks[i][j][k].ID == link.ID) {
            this.exists = true;
          }
          }
        }
      }
    },

    recieveData: function(data) {
      //console.log(this.selectedLinks);
      this.linkExists(data);
      //console.log(this.exists);
      //if exists is false then we can add to selectedLinks array
      if (this.exists == false) {
        if (data.size == 'large') {
        if (this.smallComplete == true || this.medComplete == true) {
          this.selectedLinks.push(data);
          this.layer1Index++;
          this.currLayer = 1;
          this.layer2Index = -1;
          this.layer3Index = -1;
        } else if (this.layer2Index == -1) {
          this.selectedLinks.push(data);
          this.layer1Index++; 
        }
      } else if (data.size == 'medium') {
        if (this.currLayer == 1 && this.layer2Index < 1) {
          this.smallComplete = false;
          this.selectedLinks.push([data]);
          if (this.layer2Index == -1) {
           this.layer1Index++;
          }
          this.layer2Index++;
          this.currLayer = 2;
          this.mComplete(this.layer2Index)
        } else if (this.currLayer == 2 && this.layer2Index < 1) {
          this.smallComplete = false;
          this.selectedLinks[this.layer1Index].push(data);
          this.layer2Index++;
          //currLayer = 1;
          this.mComplete(this.layer2Index)
        } else if (this.currLayer == 2 && this.layer2Index == 1) {
          this.smallComplete = false;
          this.layer2Index = 0;
          this.layer1Index++;
          this.selectedLinks.push([data]);
          this.mComplete(this.layer2Index)
        } else if (this.currLayer == 1 && this.layer2Index >= 1) {
          this.smallComplete = false;
          this.layer2Index = 0;
          this.layer1Index++;
          this.selectedLinks.push([data]);
          this.mComplete(this.layer2Index)
        } else if (this.currLayer == 3 && this.ss == false && this.layer2Index == 0) {
          this.smallComplete = false;
          this.selectedLinks[this.layer1Index].push(data);
          if (this.layer2Index == -1) {
           this.layer1Index++;
          }
          this.layer2Index++;
          this.currLayer = 2;
          this.mComplete(this.layer2Index)
          if (this.layer2Index == 1) {
            this.layer2Index = 0;
          }
        } else if (this.currLayer == 3 && this.smallComplete == true) {
          this.smallComplete = false;
          this.layer2Index = 0;
          this.currLayer = 2;
          this.layer1Index++;
          this.selectedLinks.push([data]);
          this.mComplete(this.layer2Index)
        } else if (this.currLayer == 3 && this.smallComplete == false && this.ss == false) {
          this.smallComplete = false;
          this.selectedLinks[this.layer1Index].push([data]);
          if (this.layer2Index == -1) {
           this.layer1Index++;
          }
          this.layer2Index++;
          this.currLayer = 2;
          this.mComplete(this.layer2Index)  
        } 
      } else { //when it is a small
        //4 smalls
        if (this.layer3Index == 1 && this.layer2Index == 1) {
          this.layer3Index = 0;
          this.layer2Index = -1;
          this.currLayer = 1;
        }
        //first small 
        if (this.currLayer == 1 && this.ss == false) {
          this.selectedLinks.push([[data]]);
          this.currLayer = 3;
          this.layer3Index = 0;
          this.layer2Index++;
          this.layer1Index++;
          this.sComplete(this.layer2Index, this.layer3Index);
          //the next one has to be a small
          this.ss = true;
        }
        //second small pushed after a small
        else if (this.currLayer == 3 && this.ss == true) {
          this.selectedLinks[this.layer1Index][this.layer2Index].push(data);
          if (this.smallComplete == true) {
            this.layer1Index++;  
          }
          this.currLayer = 3;
          this.layer3Index = 1;
          this.sComplete(this.layer2Index, this.layer3Index);
          //the next one can be a med or a small
          this.ss = false;
        //third small
        } else if (this.currLayer == 3 && this.ss == false) {
          //if 4 smalls have already been pushed
          if (this.layer2Index == 1) {
            this.selectedLinks[this.layer1Index].push([[data]]); 
            if (this.smallComplete == true) {
              this.layer1Index++;  
            }
            this.layer2Index = 0; 
            this.currLayer = 3;
            this.layer3Index = 0;
            this.sComplete(this.layer2Index, this.layer3Index);
            this.ss = true; 
            //if it's the 3rd small pushed in a series of 3 smalls
          } else {
            this.selectedLinks[this.layer1Index].push([data]);
            if (this.smallComplete == true) {
              this.layer1Index++;  
            }
            this.currLayer = 3;
            this.layer3Index = 0;
            this.layer2Index++;
            this.sComplete(this.layer2Index, this.layer3Index);
            this.ss = true;
          }
        } else if (this.currLayer == 2 && this.ss == false) {
          if (this.medComplete) {
	          this.selectedLinks.push([[data]]);
            this.layer2Index = 0;
            this.layer3Index = 0;
            this.layer1Index++;
            this.currLayer = 3;
            this.ss = true;
          } else {
            this.selectedLinks[this.layer1Index].push([data]);
            if (this.smallComplete == true) {
              this.layer1Index++;  
            }
            this.currLayer = 3;
            this.layer3Index = 0;
            this.layer2Index++;
            this.sComplete(this.layer2Index, this.layer3Index);
            //the next one has to be a small
            this.ss = true;
          }
        }

      };

      } else {
       console.log("Cannot add existing element");
      }
    },
  }
};
</script>
