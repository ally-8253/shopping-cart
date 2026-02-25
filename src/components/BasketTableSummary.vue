<script setup>
    import { computed } from 'vue';

    const $props = defineProps ({
        totalPrice: {
            type: Number,
            required: true
        }
    })

    const tax = computed(() => {
        const taxValue = $props.totalPrice / 10;
        return taxValue.toFixed(2);
    })
</script>

<template>
    <tr>
        <td colspan="5">
            <div class="basket-table__summary">
                <p class="basket-table__total">Total <b>${{ totalPrice }}</b></p>
                <p>Tax ${{ tax }}</p>
            </div>
        </td>
    </tr>
</template>

<style scoped>
.basket-table__summary {
  text-align: right;
}

.basket-table__total {
  font-size: 1.4rem;
}
</style>