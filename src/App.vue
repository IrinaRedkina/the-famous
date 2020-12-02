<template>
  <div class="app">
    <header class="site-header section section--header section--border-bottom">
      <div class="container">
        <div class="grid grid--middle grid--between grid--wrap">
          <Menu />
          <Search />
        </div>
      </div>
    </header>

    <main class="site-main">
      <section class="section section--catalog">
        <div class="container">
          <h1>Картины эпохи Возрождения</h1>
          <div v-if="paintings.length" class="cards grid grid--between grid--wrap">
            <Painting
              v-for="painting of paintings"
              :key="painting.id"
              :painting="painting"
              @update-status="updatePaintingStatus"
            />
          </div>
          <div v-else class="empty-text">Картин не найдено</div>
        </div>
      </section>
    </main>

    <footer class="site-footer section section--footer section--light">
      <div class="container">
        <div class="grid grid--middle grid--between grid--wrap">
          <Menu />
          <Contacts />
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
import Menu from "@/components/Menu";
import Search from "@/components/Search";
import Contacts from "@/components/Contacts";
import Painting from "@/components/Painting";

export default {
  name: 'app',
  components: {
    Menu,
    Search,
    Contacts,
    Painting
  },
  data: () => ({
    paintings: [
      {
        id: 1,
        name: 'Рождение Венеры',
        author: 'Сандро Боттичелли',
        pictureSrc: 'card-1.jpg',
        priceOld: '2000000',
        priceNew: '1000000',
        inCart: false,
        isSoldOut: false
      },
      {
        id: 2,
        name: 'Тайная вечеря',
        author: 'Леонардо да Винчи',
        pictureSrc: 'card-2.jpg',
        priceOld: '3000000',
        priceNew: null,
        inCart: false,
        isSoldOut: false
      },
      {
        id: 3,
        name: 'Сотворение Адама',
        author: 'Микеланджело',
        pictureSrc: 'card-3.jpg',
        priceOld: '6000000',
        priceNew: '5000000',
        inCart: false,
        isSoldOut: false
      },
      {
        id: 4,
        name: 'Урок анатомии',
        author: 'Рембрандт',
        pictureSrc: 'card-4.jpg',
        priceOld: '5000000',
        priceNew: null,
        inCart: false,
        isSoldOut: true
      }
    ]
  }),
  mounted() {
    if (localStorage.paintings) {
      this.paintings = JSON.parse(localStorage.getItem('paintings'))
    }
  },
  methods: {
    updatePaintingStatus(id) {
      const idx = this.paintings.findIndex(item => item.id === id)

      setTimeout(() => {
        this.paintings[idx].inCart = true
        this.updatePaintings()
      }, 1000)
    },
    updatePaintings() {
      localStorage.paintings = JSON.stringify(this.paintings)
    }
  }
}
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Merriweather:wght@300;400;700&display=swap");
@import "styles/reset.scss";
@import "styles/base.scss";
</style>