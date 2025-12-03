<template>
  <div class="box">
    <h3>{{ burger.name }}</h3>

    <img
      :src="burger.url"
      :alt="burger.name"
      style="width: 150px;"
    />

    <dl>
      <dt>{{ burger.name }}</dt>
      <dd>{{ burger.description }}</dd>
    </dl>

    <p>{{ burger.kCal }} kCal</p>

    <h4 style="font-style: italic;">Allergens</h4>
    <ul class="allergens">
      <li v-if="burger.gluten">Gluten</li>
      <li id="lactose" v-if="burger.lactose">Lactose</li>
    </ul>
    <button id="addButton" v-on:click="addBurger">
      Add a burger
    </button>
    <button id="removeButton" v-on:click="removeBurger">Remove a burger</button>
    <p>Amount ordered: {{amountOrdered}}</p>
  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      amountOrdered: 0
    }
  },
  methods: {
    addBurger: function () {
      this.amountOrdered += 1;
      this.$emit('orderedBurger', 
        {name:   this.burger.name, 
        amount: this.amountOrdered 
      }
  );
},
    removeBurger () {
      if (this.amountOrdered > 0) {
        this.amountOrdered -= 1;
        this.$emit('orderedBurger', 
        {name:   this.burger.name, 
        amount: this.amountOrdered 
      })
      }
    }
  }
}
</script>


<style scoped>
.box {
  background-color: #444;
  color: #fff;
  border-radius: 5px;
  padding: 20px;
}

.allergens {
  color: #ff5500;
}

#lactose {
  text-transform: uppercase;
}
</style>