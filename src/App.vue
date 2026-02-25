<script setup>
  import { ref, computed, onUpdated, onBeforeMount } from 'vue'
  import BasketTable from './components/BasketTable.vue';

  const basket = ref([
    {
      id: 1,
      name: 'Blue Flower Print Crop Top',
      color: 'Yellow',
      size: 'M',
      price: 29.00,
      quantity: 1,
      imageUrl: './assets/crop-top.png',
    },
    {
      id: 2,
      name: 'Levender Hoodie',
      color: 'Levender',
      size: 'XXL',
      price: 119.00,
      quantity: 1,
      imageUrl: './assets/hoodie.png',
    },
    {
      id: 3,
      name: 'Black Sweatshirt',
      color: 'Black',
      size: 'XXL',
      price: 123.00,
      quantity: 1,
      imageUrl: './assets/sweatshirt.png',
    },
  ])

  const decreaseItemQuantity = (item) => {
    item.quantity--;
  }

  const increaseItemQuantity = (item) => {
    item.quantity++;
  }

  const removeItem = (index) => {
    basket.value.splice(index, 1)
  }

  onBeforeMount(() => {
    const clientCart = localStorage.getItem("clientCart");

    if (clientCart) {
      basket.value = JSON.parse(clientCart)
    }
  })

  onUpdated(() => {
    localStorage.setItem("clientCart", JSON.stringify(basket.value))
  })
</script>

<template>
    <div class="container basket">
      <BasketTable
        :basket = "basket"
        :total-price="totalPrice"
        @decrease-item-quantity="decreaseItemQuantity"
        @increase-item-quantity="increaseItemQuantity"
        @remove-item="removeItem"
      />
    </div>
</template>

<style scoped>
  .container {
  max-width: 1200px;
  margin: 0 auto;
  background-color: #fff;
}
</style>
