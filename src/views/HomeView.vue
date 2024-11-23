<template>

<header class="header-section">
    <h1>Burger Buddies Company </h1>
    <img src="https://cdn.pixabay.com/photo/2016/11/21/16/02/outdoor-dining-1846137_1280.jpg">
  </header>
    
    <main>
      <section class="burger-section">
        <h2>Select Burger</h2>
          <p>Select your Burger Buddie here.</p>
          <div class="burger-container">
            <Burger v-for="burger in burgers" :key="burger.name" :burger="burger" v-on:orderedBurgers="addToOrder($event)"/> 
          </div>

      </section>
      <section class="information-section">
        <h2>Customer information</h2>
        <p>To deliver your Burger Buddie, we just need a bit of information from you.</p>
        <h3>
          Delivery Information
        </h3>
         <form>
            <p>
            <label for="Full name">Full Name</label><br>
            <input type="text" id="fullname" v-model="fullname" required="required" placeholder="First- and Last name">
            </p>
            <p>
            <label for="E-mail adress">E-mail</label><br>
            <input type="email" id="email" v-model="email" required="required" placeholder="E-mail address">
            </p>
            
            <p>
              <div> <label for="Gender">Gender</label><br></div>
              <div>
                <input type="radio" id="female" v-model="gender" value="female" checked="checked">
                <label for="female">Female</label>
              </div>
              <div>
                <input type="radio" id="male" v-model="gender" value="male">
                <label for="male">Male</label>
              </div>
              <div>
                <input type="radio" id="notprovided" v-model="gender" value="notprovided">
                <label for="notprovided">Do not wish to provide</label>
              </div>
            </p>
              
          </form>
      </section>

  <section class="map-section">
    <div id="map" v-on:click="addOrder" style="position: relative;">
    <div
      class="target"
      v-bind:style="{ top: location.y + 'px', left: location.x + 'px', position: 'absolute' }" 
    >
      T
    </div>
  </div>
</section>

      <section class="button-section">
        <div>
        <button type="button" v-on:click="placeOrder">
          <img src="https://img.icons8.com/emoji/48/000000/smiling-face.png" 
            alt="Smiley Face" title="Smiley Face" style="width: 10px;">
            Place my order!
        </button>
        </div>
      </section>

    </main>
        
    <footer>
      <hr>
        <p>&copy; 2024 Burger Buddies Co.</p>
    </footer>

</template>

<script>

import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io("localhost:3000");

/*create object constructur MenuItem*/
function MenuItem(name, pic, kCal, gluten, lactose) {
    this.name = name;
    this.pic = pic;
    this.kCal = kCal;
    this.gluten = gluten;
    this.lactose = lactose;
}
/*
const burgers = [
    new MenuItem("Cheesy Bacon Burger", "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTHsg5phnd-Y21zOEzmJyJtgYvTmxRd3GMwXg&s", 800, true, true),
    new MenuItem("Chicken Spicy Burger", "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTlGNtllWpnFyNGU9buKaz6xyBaHO_pj7NIKQ&s", 600, true, false),
    new MenuItem("Vego Sausy Burger", "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRCC0zPeqeRrZaG47z59tiwd_cs9JESSZtPxw&s", 500, false, true)
];
console.log(burgers); print array to the console????
*/
export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      orderedBurgers: {},
      location: { x: 0, y: 0 }
    }
  },
  methods: {
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };

      // Uppdatera platsens koordinater
      this.location.x = event.clientX - offset.x;
      this.location.y = event.clientY - offset.y;
      
    },
    setLocation: function (event) {
      var offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top,
      };
      this.location.x = event.clientX - offset.x;
      this.location.y = event.clientY - offset.y;
    },
    placeOrder: function () {
      socket.emit("addOrder", {
        orderId: this.getOrderNumber(),
        details: { x: this.location.x, y: this.location.y },
        orderItems: ["Beans", "Curry"], // Object.keys(this.orderedBurgers)
      });
    },
    markDone: function () {
      this.orderStatus = true;
    },
    addToOrder: function (event) {
      this.orderedBurgers[event.name] = event.amount;
    },
  },
};
</script>


<style>

@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');
body {
    font-family: 'Times New Roman', Times, serif;
    font-size: 12pt;
        margin: 10 px; 
        padding: 50 px;
}

h1 {
    font-family: 'Times New Roman', serif;
    font-size: 36pt;
}

.header-section {
    height: 300px;
    overflow: hidden;
    margin-top: 10px;
}
header h1 {
    position: absolute;
    width: 40rem;
    padding-left: 450px;
}
header img {
    width: 100%;
    height: 100%; /*går inte att ha auto för då visas inte bilden på rätt sätt*/
    object-fit: cover; 
    opacity: 0.5;
    object-position: 50% 30%;
}

nav ul {
    display: grid;
    grid-template-columns: repeat(auto-fill, 9.25em);
    gap: 1em;
    padding: 0;
}

nav li {
    display: block;
    background-color: grey;
    padding: 1em;
}

.Very-good {
    color: green;
}

.Master {
    color: green;
    font-weight: bold;
}

.burger-section {
    background-color: black;
    color: white;
    border: dashed white;
    padding-left: 10px;
    margin-top: 10px;
 }
 .burger-container {
    display: grid;
    grid-template-columns: 300px 300px 300px;
    gap: 5px; 
}

 .burger-item {
    margin: 2px;
}
 
 #ingredient {
    font-weight: bold;
    color: white;
 }
 .information-section{
    border: dashed black;
    padding-left: 10px;
    margin-top: 10px;
 }
 
 .button-section {
    margin: 10px;
    margin-top: 20px;
    margin-bottom: 20px;
 }

 button:hover {
    background-color: #fff700; 
    cursor: pointer; /* muspekaren blir en hand */

 }
#map {
    width: 1920px;
    height: 1078px;
    background: url("/img/polacks.jpg");
  }

.map-section {
  width: 400px; 
  height: 400px; 
  overflow: scroll; 
}

</style>