<template>
  <div v-if="localQty > 0" class="quantity_inner">
    <button @click="decreaseQty" class="bt_minus">
      <svg viewBox="0 0 24 24">
        <line x1="5" y1="12" x2="19" y2="12"></line>
      </svg>
    </button>
    <input
      type="text"
      v-model="this.localQty"
      size="2"
      class="quantity"
      data-max-count="20"
    />
    <button @click="increaseQty" class="bt_plus">
      <svg viewBox="0 0 24 24">
        <line x1="12" y1="5" x2="12" y2="19"></line>
        <line x1="5" y1="12" x2="19" y2="12"></line>
      </svg>
    </button>
  </div>
</template>

<script>
import { mapState } from "vuex";
export default {
  name: "Counter",
  props: ["qty", "id", "update"],
  data() {
    return {
      localQty: 0,
    };
  },
  created() {
    this.localQty = this.qty;
  },
  updated() {
    this.localQty = this.qty;
  },
  computed: {
    ...mapState(["list", "basket"]),
  },
  methods: {
    increaseQty() {
      this.localQty++;
      this.$store.commit("changeQty", {
        qty: this.localQty,
        id: this.id,
      });
      this.update !== undefined ? this.update() : "";
    },
    decreaseQty() {
      this.localQty--;
      this.$store.commit("changeQty", {
        qty: this.localQty,
        id: this.id,
      });
      this.update !== undefined ? this.update() : "";
    },
    setCurrentQty() {
      this.basket.forEach((item) =>
        item.id === this.id ? (this.localQty = item.qty) : ""
      );
    },
  },
};
</script>

<style scoped>
.quantity_inner * {
  box-sizing: border-box;
}
.quantity_inner {
  transform: scale(0.7);
  display: inline-flex;
  border-radius: 26px;
  border: 4px solid #d77206;
}
.quantity_inner .bt_minus,
.quantity_inner .bt_plus,
.quantity_inner .quantity {
  height: 40px;
  width: 40px;
  padding: 0;
  border: 0;
  margin: 0;
  background: transparent;
  cursor: pointer;
  outline: 0;
}
.quantity_inner .quantity {
  width: 50px;
  text-align: center;
  font-size: 30px;
  font-weight: bold;
  color: #d77206;
  font-family: Menlo, Monaco, Consolas, "Courier New", monospace;
}
.quantity_inner .bt_minus svg,
.quantity_inner .bt_plus svg {
  stroke: #d77206;
  stroke-width: 4;
  transition: 0.5s;
  margin: 10px;
}
.quantity_inner .bt_minus:hover svg,
.quantity_inner .bt_plus:hover svg {
  stroke: #000;
}
</style>
