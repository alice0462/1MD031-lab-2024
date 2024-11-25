<template>

  <div class="burger-item">
    <h3>
      {{ burger.name }}
    </h3>
    <img v-bind:src="burger.url">
    <ul>
      <p> {{ burger.kCal }} kCal</p>
    <div class="Allergies">
      <p v-if="burger.gluten"> Contains Gluten</p>
      <p v-if="burger.lactose"> Contains Lactose</p>
    </div>
      <p> Amount: {{ amountOrdered }}
        <button v-on:click="increaseOrder">+</button>
        <button v-on:click="decreaseOrder" :disabled="amountOrdered === 0">-</button>
      </p>
    </ul>
  </div>
  
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
    },
  data: function () {
  return {
    amountOrdered: 0,
    }
  },
  methods: {
    increaseOrder() {
      this.amountOrdered++; // Ökar antalet beställningar
      this.$emit('orderedBurgers', { 
        name: this.burger.name, 
        amount: this.amountOrdered 
        }
      );
    },
    decreaseOrder() {
      if (this.amountOrdered > 0) {
        this.amountOrdered--; // Minskar antalet beställningar
        this.$emit('orderedBurgers', { 
          name: this.burger.name, 
          amount: this.amountOrdered 
          }
        );
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

.burger-item {
    margin: 2px;
}
 
 .Allergies {
    font-weight: bold;
    color: white;
 }

</style>