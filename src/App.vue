<template>
  <div id="app">
    <h1>Image Ratio Helper</h1>
    <h2>a small utility to see and operate on ratio for image dimension</h2>
  
    <div>
      <input :disabled="lock.isLocked"
             type="number"
             placeholder="Width"
             v-model="width" />
      <button @click="switchDimensions">
        <img src="./assets/switch.svg" />
      </button>
      <input :disabled="lock.isLocked"
             type="number"
             placeholder="Height"
             v-model="height" />
      <button @click="toggleLock">
        <img v-if="!lock.isLocked"
             src="./assets/unlocked.svg" />
        <img v-else
             src="./assets/locked.svg" />
      </button>
      <button @click="randomFill">
        <img src="./assets/colour.svg" />
      </button>
    </div>
    <div v-if="lock.isLocked">
      <button @click="dec">-</button>
      <input type="number"
             v-model="delta">
      <button @click="inc">+</button>
    </div>
    <div id="box"
         :style="styleProperties">
      <span>made with ♥ by <a href="http://www.vikkio.it" target="_blank">vikkio</a></span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  computed: {
    styleProperties() {
      return {
        width: `${this.width}px`,
        height: `${this.height}px`,
        backgroundColor: this.colour
      };
    },
    lockIcon() {
      let action = "locked";
      if (this.lock.isLocked) {
        action = "unlocked";
      }
      return `./assets/${action}.svg`;
    }
  },
  data() {
    return {
      delta: 10,
      width: 100,
      height: 100,
      colour: "#fff",
      lock: {
        isLocked: false,
        ratio: 1
      }
    }
  },
  methods: {
    toggleLock() {
      this.lock.isLocked = !this.lock.isLocked;
      if (this.lock.isLocked) {
        this.lock.ratio = this.width / this.height;
      }
    },
    dec() {
      this.width = this.width - parseInt(this.delta);
      this.height = this.width / this.lock.ratio;
    },
    inc() {
      this.width = this.width + parseInt(this.delta);
      this.height = this.width / this.lock.ratio;
    },
    randomFill() {
      var letters = '0123456789ABCDEF';
      var color = '#';
      for (var i = 0; i < 6; i++) {
        color += letters[Math.floor(Math.random() * 16)];
      }
      this.colour = color;
    },
    switchDimensions() {
      let temp = this.width;
      this.width = this.height;
      this.height = temp;
      if (this.lock.isLocked) {
        this.lock.ratio = this.width / this.height;
      }
    }

  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#box {
  border: 1px black dashed;
  margin: auto;
  margin-top: 30px;
  border-radius: 1px;
}

span {
  margin-top: 5px;
  font-size: 8px;
}

img {
  width: 13px;
  height: 13px;
}
</style>
