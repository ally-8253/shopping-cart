<script setup>
  import { ref, computed, onUpdated, onBeforeMount } from 'vue'

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

  const totalPrice = computed(() => {
    let price = 0;

    for (let i = 0; i < basket.value.length; i++) {
      const subtotal = basket.value[i].quantity * basket.value[i].price;

      price += subtotal;
    }

    return price.toFixed(2);
  })

  const tax = computed(() => {
    const taxValue = totalPrice.value / 10;
    return taxValue.toFixed(2);
  })

  const decreaseItemQuantity = (item) => {
    item.quantity--;
  }

  const increaseItemQuantity = (item) => {
    item.quantity++;
  }

  const removeItem = (index) => {
    basket.value.splice(index, 1)
  }

  const getItemSubtotal = (item) => {
    return (item.quantity * item.price).toFixed(2)
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
      <table class="basket-table">
        <thead class="basket-table__header">
          <tr>
            <th>Product Details</th>
            <th>Price</th>
            <th>Quantity</th>
            <th>Subtotal</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody class="basket-table__body">
          <tr v-for="(item, index) in basket" :key="item.id">
            <td>
              <div class="basket-item">
                <div class="basket-item__image">
                  <img :src="item.imageUrl" :alt="item.name" />
                </div>
                <div class="basket-item__info">
                  <h2 class="basket-item__info-h2">{{ item.name }}</h2>
                  <p class="basket-item__info-p">Color: {{ item.color }}</p>
                  <p class="basket-item__info-p">Size: {{  item.size }}</p>
                </div>
              </div>
            </td>
            <td>
              <p class="basket-item__price">${{ Number(item.price).toFixed(2) }}</p>
            </td>
            <td>
              <div class="basket-item__quantity">
                <button :disabled="item.quantity === 1" @click="decreaseItemQuantity(item)" class="quantity-button">–</button>
                <input type="number" :value="item.quantity" min="1" />
                <button @click="increaseItemQuantity(item)" class="quantity-button">+</button>
              </div>
            </td>
            <td>
              <p class="basket-item__price">${{ getItemSubtotal(item) }}</p>
            </td>
            <td>
              <button @click="removeItem(index)" class="btn btn-delete" aria-label="Удалить">
                <svg
                  class="w-6 h-6 text-gray-800 dark:text-white"
                  aria-hidden="true"
                  xmlns="http://www.w3.org/2000/svg"
                  width="24"
                  height="24"
                  fill="none"
                  viewBox="0 0 24 24"
                >
                  <path
                    stroke="currentColor"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                    stroke-width="2"
                    d="M5 7h14m-9 3v8m4-8v8M10 3h4a1 1 0 0 1 1 1v3H9V4a1 1 0 0 1 1-1ZM6 7h12v13a1 1 0 0 1-1 1H7a1 1 0 0 1-1-1V7Z"
                  />
                </svg>
              </button>
            </td>
          </tr>

          <tr v-if="basket.length">
            <td colspan="5">
              <div class="basket-table__summary">
                <p class="basket-table__total">Total <b>${{ totalPrice }}</b></p>
                <p>Tax ${{ tax }}</p>
              </div>
            </td>
          </tr>

          <tr v-else>
            <td colspan="5">
              <p class="basket-table__empty">No items</p>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
</template>

<style src="./App.css"></style>
