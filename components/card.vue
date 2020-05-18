<template>
  <section>
    <!-- card  layout-->
    <button
      class="card"
      @focus="select"
      @blur="selectedCard = null, changer = false"
      :class="{'card-selected':isSelected}"
    >
      <div>
        <!-- card index -->
        <h1>#: {{id}}</h1>
        <hr>
        <!-- Card value -->
        <h1>{{randomNum}}</h1>
      </div>
    </button>
  </section>
</template>

<script>
// import - from '~/components/-.vue'

export default {
  name: "card",
  props: {
    index: {},
    id: {
      type: Number
    },
    selectedCard: {
      type: Number
    },
    randomNum: {
      type: Number
    }
  },
  data() {
    return {
      selected: false,
      changer: false
    };
  },
  computed: {
    // returns true id is equal to selected card?
    isSelected() {
      if (this.id === this.selectedCard && this.changer === true) {
        return true;
      } else {
      }
    }
  },
  methods: {
    // "emits"/ Allows the value of id from cardArray to be reached?
    select() {
      this.$emit("select", {
        id: this.id,
        randomNum: this.randomNum,
        index: this.index
      });
      this.changer = true;
    }
  }
};
</script>

<style scoped>
.card {
  width: 50px;
  height: 75px;
  background-color: cadetblue;
  margin: 25px;
  float: left;
}
.card-selected {
  width: 60px;
  height: 85px;
  background-color: crimson;
  border: 1px solid black;
  color: White;
  margin: 5px;
  float: left;
}
</style>
