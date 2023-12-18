<template>
    <div id="app">
      <div class="rectangle feature2" @click="toggleFeatureColor('f2')" :class="{ green: features.isF2Green }">
        <span class="text">Feature 2</span>
      </div>
      <div class="rectangle feature1" @click="toggleFeatureColor('f1')" :class="{ green: features.isF1Green }">
        <span class="text">Feature 1</span>
      </div>
      <div class="rectangle feature3" @click="toggleFeatureColor('f3')" :class="{ green: features.isF3Green }">
        <span class="text">Feature 3</span>
      </div> <!-- New rectangle -->
      <div class="rectangle feature4" @click="toggleFeatureColor('f4')" :class="{ green: features.isF4Green }">
        <span class="text">Feature 4</span>
      </div> <!-- New rectangle -->
      <div class="rectangle feature5" @click="toggleFeatureColor('f5')" :class="{ green: features.isF5Green }">
        <span class="text">Feature 5</span>
      </div> <!-- New rectangle -->
      <div class="rectangle base" @click="toggleBaseColor" :class="{ green: features.isBaseGreen }">
        <span class="text">Base Service</span>
      </div>
      <div class="text-box" :class="{ 'grid-item': true }">
        <p>{{ currentText }}</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        features: {
          isMid1Green: false,
          isMid2Green: false,
          isMid3Green: false,
          isMid4Green: false,
          isMid5Green: false,
          isBaseGreen: false,
        },
        currentText: 'Select Components',
        featureConfig: {
          f1: { text: 'Feature 1', dependencies: [] },
          f2: { text: 'Feature 2', dependencies: [] },
          f3: { text: 'Feature 3', dependencies: ['f1'] },
          f4: { text: 'Feature 4', dependencies: [] },
          f5: { text: 'Feature 5', dependencies: ['f2'] },
        }
      };
    },
    methods: {
      toggleFeatureColor(featureKey) {
        let feature = this.featureConfig[featureKey];
        let featureStateKey = `is${featureKey.charAt(0).toUpperCase()}${featureKey.slice(1)}Green`;
        console.log(featureStateKey)
        let order = feature.text;
  
        if (!this.features[featureStateKey]) {
          let allDependenciesActive = feature.dependencies.every(dep => this.features[`is${dep.charAt(0).toUpperCase()}${dep.slice(1)}Green`]);
  
          if (this.features.isBaseGreen && allDependenciesActive) {
            this.features[featureStateKey] = true;
            order = this.currentText + " + " + order;
          } else {
            if (!this.features.isBaseGreen) {
              this.toggleBaseColor();
            }
            feature.dependencies.forEach(dep => this.toggleFeatureColor(dep));
            this.features[featureStateKey] = true;
            order = this.currentText + " + " + order;
          }
          this.updateText(order);
        } else {
          this.features[featureStateKey] = false;
          if (featureKey === 'f2') this.disableFeature("f5");
          if (featureKey === 'f1') this.disableFeature("f3");
        }
      },
      disableFeature(featureKey) {
        let featureStateKey = `is${featureKey.charAt(0).toUpperCase()}${featureKey.slice(1)}Green`;
        this.features[featureStateKey] = false;
      },
      disableAll() {
        Object.keys(this.features).forEach(feat => {
          this.features[feat]=false;
        });
      },
      toggleBaseColor() {
        var order = "Base Service";
        if (this.features.isBaseGreen) {
            this.features.isBaseGreen = !this.features.isBaseGreen;
            this.disableAll();
            order = '';
            this.updateText(order);
        } else {
            this.features.isBaseGreen = !this.features.isBaseGreen;
            order = "Base Service";
            this.updateText(order);
        }
      },
      updateText(text) {
        this.currentText = text;
      },
    },
  };

//  export default {
//    data() {
//      return {
//        isMid1Green: false,
//        isMid2Green: false,
//        isMid3Green: false,
//        isMid4Green: false,
//        isMid5Green: false,
//        isBaseGreen: false,
//        currentText: 'Select Components',
//      };
//    },
//    methods: {
//      toggleMid2Color() {
//        var order = "Feature 2";
//        if (!this.isMid2Green){
//            if (this.isBaseGreen){
//                this.isMid2Green = true;
//                order = this.currentText + " + " + order;
//                this.updateText(order);
//            } else {
//                this.isBaseGreen = true
//                this.isMid2Green = true
//                order = "Base Service" + " + " + order;
//                this.updateText(order);
//            }
//        } else {
//            this.isMid2Green = false
//            this.disableMid5();
//        }
//      },
//      toggleMid4Color() {
//        var order = "Feature 4";
//        if (!this.isMid4Green){
//            if (this.isBaseGreen){
//                this.isMid4Green = true;
//                order = this.currentText + " + " + order;
//                this.updateText(order);
//            } else {
//                this.isBaseGreen = true
//                this.isMid4Green = true
//                order = "Base Service" + " + " + order;
//                this.updateText(order);
//            }
//        } else {
//            this.isMid4Green = false
//            // No feature on top this.disableMid3();
//        }
//      },
//      toggleMid1Color() {
//        var order = "Feature 1";
//        if (!this.isMid1Green){
//            if (this.isBaseGreen){
//                this.isMid1Green = true;
//                order = this.currentText + " + " + order;
//                this.updateText(order);
//            } else {
//                this.isBaseGreen = true
//                this.isMid1Green = true
//                order = "Base Service" + " + " + order;
//                this.updateText(order);
//            }
//        } else {
//            this.isMid1Green = false
//            this.disableMid3();
//        }
//      },
//      toggleMid3Color() { // New method
//        var order = "Feature 3";
//        if (!this.isMid3Green){
//            if (this.isBaseGreen && this.isMid1Green){
//              this.isMid3Green = true;
//              order = this.currentText + " + " + order;
//              this.updateText(order);
//            } else {
//              this.isBaseGreen = true;
//              this.isMid1Green = true;
//              this.isMid3Green = true;
//              order = "Base Service + Feature 1" + " + " + order;
//              this.updateText(order);
//            }
//        } else {
//          this.isMid3Green = false;
//        }
//      },
//      toggleMid5Color() { // New method
//        var order = "Feature 5";
//        if (!this.isMid5Green){
//            if (this.isBaseGreen && this.isMid2Green){
//              this.isMid5Green = true;
//              order = this.currentText + " + " + order;
//              this.updateText(order);
//            } else {
//              this.isBaseGreen = true;
//              this.isMid2Green = true;
//              this.isMid5Green = true;
//              order = "Base Service + Feature 2" + " + " + order;
//              this.updateText(order);
//            }
//        } else {
//          this.isMid5Green = false;
//        }
//      },
//      disableMid3() {
//        this.isMid3Green = false;
//      },
//      disableMid5() {
//        this.isMid5Green = false;
//      },
//      disableAll() {
//        this.isMid1Green = false;
//        this.isMid2Green = false;
//        this.isMid3Green = false;
//        this.isMid4Green = false;
//        this.isMid5Green = false;
//      },
//      toggleBaseColor() {
//        var order = "Base Service";
//        if (this.isBaseGreen) {
//            this.isBaseGreen = !this.isBaseGreen;
//            this.disableAll();
//            order = '';
//            this.updateText(order);
//        } else {
//            this.isBaseGreen = !this.isBaseGreen;
//            order = "Base Service";
//            this.updateText(order);
//        }
//      },
//      updateText(text) {
//        this.currentText = text;
//      },
//    },
//  };
  </script>
  
<style>
  #app {
    display: grid;
    justify-content: center; /* Horizontally center the grid */
    padding: 20px; /* Adds some space around the grid */
    gap: 10px; /* Space between grid items */

    grid-template-columns: repeat(5, 100px); /* Adjust as needed */
    grid-template-rows: repeat(3, 100px); /* Adjust as needed */
  }

  .rectangle {
    height: 100px;
    background-color: blue;
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .rectangle.green {
    background-color: green;
  }

  .feature1 {
    grid-column: 2;
    grid-row: 2;
    width: 100px;
  }

  .feature2 {
    grid-column: 1;
    grid-row: 2;
    width: 100px;
  }

  .feature3 {
    grid-column: 2;
    grid-row: 1;
    width: 100px;
  }

  .feature4 {
    grid-column: 3;
    grid-row: 2;
    width: 100px;
  }

  .feature5 {
    grid-column: 1;
    grid-row: 1;
    width: 100px;
  }

  .base {
    grid-row: 3;
    grid-column: 1 / span 3; /* Makes the base rectangle span all columns */
    width: 320px; /* Adjust width to match the total width of the above rectangles including gaps */
  }

  .text {
    color: white; /* Choose a color that contrasts well with the rectangle colors */
    font-size: 16px; /* Adjust font size as needed */
  }

  .text-box {
    grid-column: 4 / span 2; /* Span 4th and 5th columns */
    grid-row: 1 / span 3; /* Span all rows */
    border: 1px solid #ccc; /* Style as needed */
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>