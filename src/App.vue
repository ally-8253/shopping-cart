<script setup>
  import { ref, computed, onUpdated, onBeforeMount } from 'vue'
  import BasketTable from './components/BasketTable.vue';
  import BasketTableSummary from './components/BasketTableSummary.vue';

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
            <BasketTable
              :basket = "basket"
              @decrease-item-quantity="decreaseItemQuantity"
              @increase-item-quantity="increaseItemQuantity"
              @remove-item="removeItem"
            />

            <tr v-if="basket.length">
            <td colspan="5">
              <BasketTableSummary
                :total-price="totalPrice"
              />
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

<style scoped>
  .container {
  max-width: 1200px;
  margin: 0 auto;
  background-color: #fff;
}

.basket-table {
  width: 100%;
  border-collapse: collapse;
}

.basket-table__header {
  background-color: #3c4242;
  color: #fff;
  font-weight: 400;
  text-transform: uppercase;
}

.basket-table__header th {
  padding: 2rem 1rem;
  font-weight: 400;
  text-align: center;
  border: 0;
}

.basket-table__header th:first-child {
  text-align: left;
  padding-left: 5rem;
}

.basket-table__header th:last-child {
  padding-right: 5rem;
}


.basket-table__empty {
  text-align: center;
  color: #a7a7a7;
}


</style>
