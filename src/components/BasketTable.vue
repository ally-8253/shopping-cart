<script setup>
    import BasketTableItem from './BasketTableItem.vue';

    defineProps({
        basket: {
            type: Array,
            required: true
        }
    })

    defineEmits(['decreaseItemQuantity', 'increaseItemQuantity', 'removeItem'])
</script>

<template>
    <tr v-for="(item, index) in basket" :key="item.id">
        <BasketTableItem
            :item="item"
            :index="index"
            @decrease-item-quantity="$emit('decreaseItemQuantity', item)"
            @increase-item-quantity="$emit('increaseItemQuantity', item)"
            @remove-item="$emit('removeItem', index)"
        />
    </tr>
</template>

<style scoped>
    .basket-table__body td {
    padding: 3rem 2rem;
    text-align: center;
    }

    .basket-table__body td:first-child {
    text-align: left;
    padding-left: 5rem;
    }

    .basket-table__body td:last-child {
    padding-right: 5rem;
    }
</style>