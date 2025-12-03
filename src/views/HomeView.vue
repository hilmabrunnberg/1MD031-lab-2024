<template>
  <!-- Sending messages (HTML) -->
  <div>
    <header>
      <h1 id="WelcomeText">Welcome to Hilma's burgers</h1>
      <img
        src="/img/BurgerBackground.avif"
        alt="Inside a burger restaurant"
        id="header-image"
      />
    </header>

    <main>
      <!-- Burger secion -->
      <section id="burgerselection">
        <h2 style="text-align: center;">Select burger</h2>
        <p style="text-align: center;">
          This is where you execute your burger selection. All burgers can be made
          with a bread that is gluten- free and lactose free.
        </p>

        <div class="wrapper">
          <Burger
            v-for="burger in menu"
            v-bind:key="burger.name"
            v-bind:burger="burger"
            v-on:orderedBurger="addToOrder"
          />
        </div>
      </section>

      <!-- Customer information-->
      <section id="customerinformation">
        <form>
          <h2>Customer information</h2>
          <p>This is where you provide necessary information</p>

          <p>
            <label for="fullName">Full name</label><br />
            <input
              type="text"
              id="fullName"
              v-model="fn"
              placeholder="First- and Last name"
              required="required"
            />
          </p>

          <p>
            <label for="email">Email</label><br />
            <input
              type="email"
              id="email"
              v-model="em"
              placeholder="E-mail adress"
              required="required"
            />
          </p>

          <h3>Enter your gender</h3>
          <p>
            <input type="radio" id="man" v-model="gender" value="man" />
            <label for="man">Man</label><br />
            <input type="radio" id="woman" v-model="gender" value="woman" />
            <label for="woman">Woman</label><br />
            <input type="radio" id="other" v-model="gender" value="other" />
            <label for="other">Do not wish to provide</label><br />
          </p>

          <p>
            <label for="paymentMethod">Payment method</label><br />
            <select id="paymentmethod" v-model="pm">
              <option selected="selected">Swish</option>
              <option>Bank transfer</option>
              <option>Klarna</option>
            </select>
          </p>

          <button type="button" v-on:click="placeOrder">
            <img src="/img/checkmark.png" style="width: 20px;" />
            <img src="/img/cartoonBurger.jpg" style="width: 20px;" />
            Send information
          </button>
        </form>
      </section>
      <section id="mapSection">
  <div id="map" @click="setLocation">
    <div
      id="mapDot"
      :style="{ left: location.x + 'px', top: location.y + 'px' }"
    >
      T
    </div>
  </div>
</section>
    </main>

    <footer style="color: grey;">
      <hr />
      <p>&copy; Hilmas Hamburgare AB</p>
      Customer service contact information:
      <br />
      Phone number: +46 72 214 57 72
      <br />
      email: hilma.brunnberg@gmail.com
    </footer>
  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue'
import io from 'socket.io-client'
import menu from '../assets/menu.json'

const socket = io('localhost:3000')

export default {
  name: 'HomeView',
  components: {
    Burger
  },
  data () {
    return {
      menu,

      fn: '',
      em: '',
      str: '',
      hn: null,
      gender: '',
      pm: 'Swish',
      orderedBurgers: {},
      location: { x: 0,
            y: 0
          }
    }
  },
  methods: {
    placeOrder () {
      console.log('New order from customer:')
      console.log('Name:', this.fn)
      console.log('Email:', this.em)
      console.log('Gender:', this.gender)
      console.log('Payment method:', this.pm)
      console.log('Order summary:', this.orderedBurgers)
      
      socket.emit('addOrder', {
        orderId: this.getOrderNumber(),
        details: {
                  x: this.location.x,
                  y: this.location.y,
        },
        customerName: this.fn,
        email: this.em,
        paymentMethod: this.pm,
        orderItems: this.orderedBurgers
      })
    },
    setLocation(event){
      const offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };
         this.location.x = event.clientX - 10 - offset.x;
         this.location.y = event.clientY - 10 - offset.y;
    },

    getOrderNumber () {
      return Math.floor(Math.random() * 100000)
    },
    addToOrder(event) {
      this.orderedBurgers[event.name] = event.amount;
      console.log("Updated order:", this.orderedBurgers);
    },

    addOrder (event) {
      const offset = {
        x: event.currentTarget.getBoundingClientRect().left,
        y: event.currentTarget.getBoundingClientRect().top
      };
         this.location.x = event.clientX - 10 - offset.x;
         this.location.y = event.clientY - 10 - offset.y;
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Agbalumo&family=Cormorant:wght@700&display=swap');

body {
  font-size: 14pt;
  font-family: Arial, sans-serif;
}

p {
  color: rgb(80, 79, 79);
}

.box p {
  color: #ffffff;
}

#WelcomeText {
  font-family: 'Courier New';
  font-size: 36pt;
  position: absolute;
  top: 30%;
  left: 25%;
}

header {
  height: 300px;
  overflow: hidden;
  padding-bottom: 50px;
  position: relative;
}

main,
header,
footer,
nav ul {
  max-width: 100vw;
  margin: 0 auto 0 auto;
}

#header-image {
  width: 100%;
  height: auto;
  opacity: 0.7;
}

main {
  background-color: lightblue;
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

button:hover {
  background-color: rgb(74, 74, 203);
  cursor: pointer;
}

#burgerselection {
  background-color: black;
  color: white;
}

section {
  padding: 5px;
  margin: 10px;
  border: dashed 2px;
}

button {
  margin-bottom: 10px;
}

div {
  padding: 5px;
  margin: 10px;
}

.wrapper {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: 33% 33% 33%;
  background-color: #fff;
  color: #444;
}

.box {
  background-color: #444;
  color: #fff;
  border-radius: 5px;
  padding: 20px;
}
#map {
  position:relative;
  width: 1920px;
  height:1978px;
  background: url("/img/polacks.jpg");  margin: 20px auto;
  cursor: crosshair;
}
#mapSection{
  overflow: scroll;
  max-height: 70vh;
}
  #mapDot {
    position: absolute;
    background: black;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }

</style>
