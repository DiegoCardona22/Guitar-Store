<script setup>
import Footer from "./components/Footer.vue";
import Guitar from "./components/Guitar.vue";
import Header from "./components/Header.vue";
import { db } from "./data/guitars";
import { ref, onMounted, watch } from "vue";

const guitars = ref([]);
const cart = ref([]);
const guitar = ref({});

watch(cart, () => {
  saveCartLS();
}, { deep: true });

const obtainCartLS = () => {
  const cartLS = localStorage.getItem("cart");

  if (cartLS) {
    cart.value = JSON.parse(cartLS);
  }
};

onMounted(() => {
  guitars.value = db;
  guitar.value = db[3];
  obtainCartLS();
});

const saveCartLS = () => {
  localStorage.setItem("cart", JSON.stringify(cart.value));
};

const addToCart = (guitar) => {
  const exists = cart.value.findIndex((item) => item.id === guitar.id);

  if (exists >= 0) {
    cart.value[exists].quantity++;
  } else {
    guitar.quantity = 1;
    cart.value.push(guitar);
  }
};

const decrementQuantity = (guitar) => {
  const exists = cart.value.findIndex((item) => item.id === guitar.id);

  if (exists >= 0) {
    cart.value[exists].quantity--;

    if (cart.value[exists].quantity === 0) {
      cart.value.splice(exists, 1);
    }
  }
};

const incrementQuantity = (guitar) => {
  const exists = cart.value.findIndex((item) => item.id === guitar.id);

  if (exists >= 0) {
    cart.value[exists].quantity++;
  }
};

const deleteFromCart = (id) => {
  const exists = cart.value.filter((item) => item.id !== id);
  cart.value = exists;
};

const emptyCart = () => {
  cart.value = [];
};

</script>

<template>
  <Header 
    v-bind:cart="cart"
    v-bind:guitar="guitar"
    v-on:decrement-quantity="decrementQuantity"
    v-on:increment-quantity="incrementQuantity"
    v-on:add-to-cart="addToCart"
    v-on:delete-from-cart="deleteFromCart"
    v-on:empty-cart="emptyCart"
  />

  <main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
      <Guitar
        v-for="guitar in guitars"
        v-bind:guitar="guitar"
        v-on:add-to-cart="addToCart"
      />
      <!-- FIN guitar -->
    </div>
  </main>

  <Footer />
</template>

<style scoped></style>
