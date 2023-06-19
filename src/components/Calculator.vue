<template>
  <div>
    <button size="lg" variant="success" @click="clearFields">Wyczyść</button>
    <div>
      <label>średnica [mm]</label>
      <select
        id="diameter"
        v-model="diameter"
        label="diameter"
        size="sm"
        class="mt-3"
      >
        <!-- eslint-disable-next-line vue/require-v-for-key -->
        <option v-for="option in diameterOptions" :value="option.value">
          {{ option.text }}
        </option>
      </select>
      <div v-if="isOtherDiameter() || otherDiameterSelected">
        <label>podaj średnicę [mm]</label>
        <input id="otherDiameterVal" v-model="diameter" />
      </div>
      <label>masa [g]</label>
      <input id="mass" v-model="mass" />
      <label>wysokość [mm]</label>
      <input id="height" v-model="height" />
      <label>gęstość [g/cm^3]</label>
      <input id="density" v-model="density" />
    </div>
    <button size="lg" variant="success" @click="calculate">Oblicz</button>
    <div v-if="this.result !== null">
      <p>WYNIK: {{ this.result }}</p>
    </div>
  </div>
</template>

<script>
/* eslint-disable vue/require-v-for-key */

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "Calculator",
  data: function () {
    return {
      result: null,
      diameter: null,
      mass: null,
      height: null,
      density: null,
      otherDiameterSelected: false,
      diameterOptions: [
        { value: 0, text: "inna" },
        { value: 10, text: "10" },
        { value: 13, text: "13" },
        { value: 16, text: "16" },
        { value: 20, text: "20" },
        { value: 25, text: "25" },
        { value: 30, text: "30" },
        { value: 36, text: "36" },
        { value: 37, text: "37" },
        { value: 40, text: "40" },
        { value: 50, text: "50" },
      ],
    };
  },
  methods: {
    isOtherDiameter() {
      const cond = this.diameter === 0;
      if (cond) {
        this.otherDiameterSelected = true;
      }
      return cond;
    },
    clearFields() {
      this.mass = null;
      this.density = null;
      this.height = null;
      this.result = null;
      this.diameter = null;
      this.otherDiameterSelected = false;
    },
    calculate() {
      let result;
      if (this.mass === null) {
        result = this.calculateMass();
        this.mass = result.toFixed(2);
      } else if (this.height === null) {
        result = this.calculateHeight();
        this.height = result.toFixed(2);
      } else if (this.density === null) {
        result = this.calculateDensity();
        this.density = result.toFixed(2);
      } else {
        alert("wielkość obliczana powinna być pusta");
      }
      result = result.toFixed(2);
      console.log(result);
      this.result = result;
      return result;
    },
    calculateMass() {
      const area = this.calculateArea();
      const density = this.replaceComma(this.density);
      const height = this.replaceComma(this.height);
      let mass = (area * height * density) / 10;
      mass = mass / 100;
      return mass;
    },
    calculateHeight() {
      const area = this.calculateArea();
      const mass = this.replaceComma(this.mass);
      const density = this.replaceComma(this.density);
      let height = (10 * mass) / density / area;
      height = height * 100;
      return height;
    },
    calculateDensity() {
      const area = this.calculateArea();
      const mass = this.replaceComma(this.mass);
      const height = this.replaceComma(this.height);
      let density = (10 * mass) / (area * height);
      density = density * 100;
      return density;
    },
    calculateArea() {
      const diameter = this.replaceComma(this.diameter);
      const radius = diameter / 2;
      const area = Math.PI * Math.pow(radius, 2);
      return area;
    },
    replaceComma(x) {
      return x.toString().replace(/,/g, ".");
    },
  },
};
</script>

<style scoped>
div {
  display: grid;
}
button {
  margin-top: 10px;
}
p {
  padding-top: 10px;
}
b-form-input {
  padding-top: 5px;
}
label {
  padding-top: 5px;
}
</style>
