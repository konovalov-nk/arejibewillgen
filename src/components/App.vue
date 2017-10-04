<template>
  <div id="app">
    <header>
      Basically, rainbow color wheel generator with any number of steps.
    </header>
    <main>
      <div id="wrapper">
        <div id="user-input">
          <input v-on:keyup.enter="makeRainbowSteps" v-model="current_keys" type="text" placeholder="How many keys are on your keyboard?" />
          <button v-on:click="makeRainbowSteps">Generate</button>
        </div>
        <div id="results">
          <div class="color" v-for="color in colors">
            <span :style="`background-color: rgb(${color.r}, ${color.g}, ${color.b});`" class="circle"></span>
            <pre>{{ getColorString(color) }}</pre>
          </div>
        </div>
      </div>
    </main>
    <footer></footer>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      current_keys: 104,
      colors: []
    }
  },
  
  methods: {
    makeRainbowSteps: function () {
      let hsl

      this.colors = []
      for (let i = 0; i < this.current_keys; i++) {
        hsl = this.hslToRgb(parseFloat(i) * (1.0 / parseFloat(this.current_keys)), 1, 0.5)
        this.colors.push({r: hsl[0], g: hsl[1], b: hsl[2]})
      }
    },
    
    getColorString: function (color) {
      return `R: ${color.r.toString().padStart(3)}, G: ${color.g.toString().padStart(3)}, B: ${color.b.toString().padStart(3)}`
    },
    
    hslToRgb: function(h, s, l) {
        let r, g, b

        if (s == 0) {
            r = g = b = l // achromatic
        } else {
            let hue2rgb = function hue2rgb(p, q, t) {
                if(t < 0) t += 1
                if(t > 1) t -= 1
                if(t < 1/6) return p + (q - p) * 6 * t
                if(t < 1/2) return q
                if(t < 2/3) return p + (q - p) * (2/3 - t) * 6
                return p
            }

            let q = l < 0.5 ? l * (1 + s) : l + s - l * s
            let p = 2 * l - q
            r = hue2rgb(p, q, h + 1/3)
            g = hue2rgb(p, q, h)
            b = hue2rgb(p, q, h - 1/3)
        }

        return [Math.round(r * 255), Math.round(g * 255), Math.round(b * 255)]
    }
  },
  
  mounted: function () {
    this.makeRainbowSteps()
  }
}
</script>

<style>
  @import "../style/normalize.css";
  
  body {
    margin: 0;
  }
  
  main {
    padding: 1rem;
  }
  
  header {
    padding: 1rem;
    background: whitesmoke;
  }
  
  button {
    padding: .5rem 1rem;
    border: 1px solid #ddd;
    background: transparent;
  }
  
  .circle {
    width: 15px;
    height: 15px;
    border-radius: 7px;
    display: inline-block;
  }
  
  #wrapper {
    margin: 0 auto;
    max-width: 50%;
    padding: 1em;
  }
  
  #results {
    margin-top: 1em;
    text-align: center;
  }
  
  #user-input {
    text-align: center;
  }
  
  input {
    width: 17.5em;
    padding: 0.5em;
    border: 1px solid #ccc;
  }
  
  pre {
    display: inline-block;
    margin: 0;
  }
  
  .color {
    margin-bottom: 0.25em;
  }
</style>