<template>
  <div :data-id="painting.id" class="card" :class="{ 'card--sales': painting.isSoldOut }">
    <a href="#" class="card__img-link">
      <img :src="'images/'+painting.pictureSrc" class="card__img" :alt="painting.name">
    </a>
    <div class="card__info">
      <a href="#" class="card__link">&laquo;{{ painting.name }}&raquo;</a>
      <div class="card__author">{{ painting.author }}</div>
      <div v-if="!painting.isSoldOut" class="card__shop-panel">
        <div class="card__price">
          <span v-if="painting.priceNew">
            <span class="card__old-price">{{ formatToUSD(painting.priceOld) }}</span>
            <span class="card__new-price">{{ formatToUSD(painting.priceNew) }}</span>
          </span>
          <span v-else>
            <span class="card__new-price">{{ formatToUSD(painting.priceOld) }}</span>
          </span>
        </div>
        <button
          type="button"
          class="button card__button"
          :class="{ 'button--success': inCart }"
          :disabled="loading || inCart"
          @click.prevent="onClickBuyButton"
        >
          <Loader v-if="loading" />
          <span v-else>
            {{ textButton }}
          </span>
        </button>
      </div>
      <div v-else class="card__shop-panel">
        <p class="sales-text">Продана на аукционе</p>
      </div>
    </div>
  </div>
</template>

<script>
import Loader from "@/components/Loader";
import axios from "axios";

export default {
  name: 'painting',
  components: {
    Loader
  },
  props: {
    painting: {
      type: Object,
      required: true
    }
  },
  data: () => ({
    loading: false
  }),
  computed: {
    textButton() {
      return this.painting.inCart ? 'В корзине' : 'Купить'
    },
    inCart() {
      return this.painting.inCart
    }
  },
  methods: {
    formatToUSD(value, currency = `USD`) {
      return new Intl.NumberFormat(`en-En`, {
        style: `currency`,
        currency,
        minimumFractionDigits: 0,
      }).format(value);
    },
    onClickBuyButton() {
      this.loading = true

      axios.get('https://jsonplaceholder.typicode.com/posts/1')
        .then((response) => {
          if (response.status === 200 || response.status === 201) {
            this.$emit('update-status', this.painting.id)

            setTimeout(() => {
              this.loading = false
            }, 1000)
          }
        })
        .catch((error) => {
          setTimeout(() => {
            alert('Ошибка '+ error)
            this.loading = false
          }, 2000)
        })
    }
  },
}
</script>

<style lang="scss" scoped>
@import "../styles/card.scss";
</style>