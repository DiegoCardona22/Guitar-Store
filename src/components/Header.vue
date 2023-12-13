<script setup>
import { computed } from "vue";
const props = defineProps({
  cart: {
    type: Array,
    required: true,
  },
  guitar: {
    type: Object,
    required: true,
  },
});

const total = computed(() => {
  return props.cart.reduce((acc, item) => acc + (item.quantity * item.price), 0);
});

defineEmits(["decrement-quantity", "increment-quantity", "add-to-cart", "delete-from-cart", "empty-cart"]);
</script>

<template>
  <header class="py-5 header">
    <div class="container-xl">
      <div class="row justify-content-center justify-content-md-between">
        <div class="col-8 col-md-3">
          <a href="index.html">
            <img class="img-fluid" src="/img/logo.svg" alt="imagen logo" />
          </a>
        </div>
        <nav
          class="col-md-6 a mt-5 d-flex align-items-start justify-content-end"
        >
          <div class="carrito">
            <img
              class="img-fluid"
              src="/img/carrito.png"
              alt="imagen carrito"
            />

            <div id="carrito" class="bg-white p-3">
              <p class="text-center" v-if="cart.length === 0">
                El carrito esta vacio
              </p>
              <p class="text-center" v-else>
                Productos en el carrito: {{ cart.length }}
              </p>
              <div v-if="cart.length > 0">
                <table class="w-100 table">
                  <thead>
                    <tr>
                      <th>Imagen</th>
                      <th>Nombre</th>
                      <th>Precio</th>
                      <th>Cantidad</th>
                      <th></th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="product in cart">
                      <td>
                        <img
                          class="img-fluid"
                          v-bind:src="`/img/${product.image}.jpg`"
                          v-bind:alt="product.name"
                        />
                      </td>
                      <td>
                        {{ product.name }}
                      </td>
                      <td class="fw-bold">${{ product.price }}</td>
                      <td class="flex align-items-start gap-4">
                        <button type="button" class="btn btn-dark" v-on:click="$emit('decrement-quantity', product)">-</button>
                        {{ product.quantity }}
                        <button type="button" class="btn btn-dark" v-on:click="$emit('increment-quantity', product)">+</button>
                      </td>
                      <td>
                        <button class="btn btn-danger" type="button" v-on:click="$emit('delete-from-cart', product.id)">
                          X
                        </button>
                      </td>
                    </tr>
                  </tbody>
                </table>

                <p class="text-end">
                  Total pagar: <span class="fw-bold">${{ total }}</span>
                </p>
                <button class="btn btn-dark w-100 mt-3 p-2" type="button" v-on:click="$emit('empty-cart')">
                  Vaciar Carrito
                </button>
              </div>
            </div>
          </div>
        </nav>
      </div>
      <!--.row-->

      <div class="row mt-5">
        <div class="col-md-6 text-center text-md-start pt-5">
          <h1 class="display-2 fw-bold">Modelo {{ guitar.name }}</h1>
          <p class="mt-5 fs-5 text-white">
            {{ guitar.description }}
          </p>
          <p class="text-primary fs-1 fw-black">${{ guitar.price }}</p>
          <button
            type="button"
            class="btn fs-4 bg-primary text-white py-2 px-5"
            v-on:click="$emit('add-to-cart', guitar)"
          >
            Agregar al Carrito
          </button>
        </div>
      </div>
    </div>

    <img
      class="header-guitar"
      src="/img/header_guitar.png"
      alt="imagen header"
    />
  </header>
</template>
