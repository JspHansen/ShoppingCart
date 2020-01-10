<template lang="pug">
  div(id="app")
    div.app-container.color-primary
      .product-list(v-if="products.length")
        div.product-list__radio-wrapper(
          v-for="(item, index) in products"
          :key="index"
          @click="(e) => onSelectProduct(item)"
        )
          div.product-list__radio
            input(
              type="radio"
              name="product-option"
              :id="item.id"
              :checked="item.id === product.id"
              :value="item.id"
            )
            label(:for="item.id")
              span.label__name {{ `LICENSE PLAN #${ item.id }` }}
              span.label__price {{ `$${ item.price } per license` }}

      .product-quantity
        span Number of licenses:
        select(:value="quantity" @change="onSelectQuantity")
          option(v-for="i in product_quantities" :key="i" :value="i") {{ i }}

      .product-total
        p.price TOTAL: <span class="color-secondary">${{ total }}<sup>us</sup></span>
        p.button BUY NOW
        p.plan.color-secondary Selected Plan: {{ `#${ product.id }` }}

</template>

<script>
import ProductsJson from '@/json/products.json'
import QuantitiesJson from '@/json/quantities.json'

export default {
  name: 'App',

  data: () => ({
    products: ProductsJson,
    product: {
      id: '',
      price: 0
    },
    quantities: QuantitiesJson,
    quantity: 1,
    total: ''
  }),

  computed: {
    product_quantities () {
      let quantity = this.quantities.find(q => q.product_id === this.product.id)

      return quantity
        ? quantity.quantities
        : []
    }
  },

  methods: {
    onSelectProduct (item) {
      this.product = item;

      this.setTotalPrice();
    },

    onSelectQuantity (e) {
      this.quantity = parseInt(e.target.value, 10);

      this.setTotalPrice();
    },

    setTotalPrice () {
      if (
        !this.product.price ||
        !this.quantity
      ) {
        return;
      }

      let total = this.quantity * this.product.price;

      this.total = !isNaN(total)
        ? total
        : '';
    }
  }
}
</script>
