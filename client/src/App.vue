<template>
  <div id="app" v-cloak>
    <Navigation />

    <BaseLoadingSpinner v-if="categoriesLoadingStatus" />

    <transition name="slide-in" mode="out-in">
      <router-view :key="$route.fullPath" />
    </transition>

    <CategoriesMenuToggle />

    <CategoriesMenu :categories="categories" />
  </div>
</template>

<script>
import { mapGetters } from 'vuex';
const Navigation = () => import('./components/layout/Navigation');
import CategoriesMenuToggle from './components/layout/CategoriesMenuToggle';
import CategoriesMenu from './components/layout/CategoriesMenu';

export default {
  name: 'App',
  components: {
    Navigation,
    CategoriesMenuToggle,
    CategoriesMenu
  },
  computed: {
    ...mapGetters(['categoriesLoadingStatus', 'categoriesError', 'categories'])
  },
  beforeCreate() {
    this.$store.commit('INITIALISE_CART');
  },
  async created() {
    if (this.categories && this.categories.length > 0) return;
    await this.$store.dispatch('fetchCategories');
  }
};
</script>

<style lang="scss">
:root {
  --white: rgba(255, 255, 255, 1);
  --blue: rgba(44, 101, 255, 1);
  --blue-disabled: rgba(44, 101, 255, 0.4);
  --gray: rgba(240, 240, 240, 1);
  --black: rgba(0, 0, 0, 0.9);
  --shadow: rgba(0, 0, 0, 0.2);
}

.ovHidden {
  overflow: hidden;
}

.section {
  &__details {
    display: flex;
    justify-content: space-between;
    align-items: center;
    text-transform: uppercase;
    margin-bottom: 3rem;
    @media only screen and (max-width: 48em) {
      flex-direction: column;
      align-items: flex-start;
      padding: 2rem 0 0;
    }
    @media only screen and (max-width: 34.125em) {
      display: none;
    }
  }

  &__title {
    font-size: 6.2rem;
    font-weight: 300;
    margin-right: 5rem;
    @media only screen and (max-width: 160em) {
      font-size: 5.2rem;
    }
    @media only screen and (max-width: 64em) {
      font-size: 4.5rem;
    }
    @media only screen and (max-width: 48em) {
      font-size: 4.25rem;
    }
  }

  &__category {
    font-size: 1.75rem;
    font-weight: 300;
    color: rgba(0, 0, 0, 1);
  }
}
.category__main {
  width: 100%;
  min-height: 100vh;
  padding: 8rem 25rem 2rem 39.5rem;
  background-color: var(--gray);
  @media only screen and (max-width: 64em) {
    padding: 8rem 13rem;
  }
  @media only screen and (max-width: 48em) {
    padding: 8rem 10rem;
  }
  @media only screen and (max-width: 34.125em) {
    padding: 8rem;
  }
  @media only screen and (max-width: 25em) {
    padding: 10rem 3rem;
  }
}

.btn__load-more {
  display: block;
  border: 0;
  margin: 2rem auto;
  padding: 2rem;
  background: none;
  text-transform: uppercase;
  color: var(--blue);
  cursor: pointer;
  font-size: 1rem;
  font-weight: 700;
  transition: all 0.4s;
}

.close {
  position: absolute;
  top: 8rem;
  left: 8rem;
  width: 3rem;
  height: 3rem;
  background: none;
  border: 0;
  cursor: pointer;
  font-size: 3rem;
  transition: 0.2s all;
  z-index: 12;
  @media only screen and (max-width: 48em) {
    font-size: 3.5rem;
  }
  @media only screen and (max-width: 34.125em) {
    top: 3.5rem;
    left: 4rem;
  }

  &:hover {
    color: rgba(0, 35, 255, 0.9);
  }
}

.close-cart,
.close-search {
  left: 24rem;
  @media only screen and (max-width: 48em) {
    left: 2rem;
  }
}

.input__quantity {
  width: 10rem;
  padding: 1rem 1rem;
  border: 1px solid transparent;
  border-radius: 2.5rem;
  text-align: center;
  font-weight: 700;
  font-size: 1.75rem;
  -moz-appearance: textfield;

  &::-webkit-inner-spin-button,
  &::-webkit-outer-spin-button {
    -webkit-appearance: none;
    margin: 0;
  }
}

.search {
  &__input {
    font-family: 'Lato', sans-serif;
    font-size: 4rem;
    font-weight: 300;
    text-transform: uppercase;
    border: 0;
    border-bottom: 1px solid rgba(177, 177, 177, 0.9);
    background-color: transparent;
    padding: 1.5rem;
    width: 70%;
    margin-bottom: 1rem;
    @media only screen and (max-width: 64em) {
      width: 100%;
      padding: 0;
    }

    &:focus {
      outline: 2px solid var(--blue);
    }
  }

  &__label {
    font-size: 1.8rem;
    color: rgba(0, 0, 0, 0.7);
  }
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
}

.slide-in-enter-active {
  transition: all 0.3s ease;
}

.slide-in-leave-active {
  transition: all 0.3s cubic-bezier(1, 0.5, 0.8, 1);
}

.slide-in-enter,
.slide-in-leave-to {
  opacity: 0;
  transform: translateX(-100%);
}

.slide-fade-enter-active,
.slide-fade-leave-active {
  transition: all 0.4s ease;
}

.slide-fade-enter {
  transform: translateY(-100%);
  opacity: 0;
}

.slide-fade-leave-to {
  transform: translateY(-100%);
}

.list-enter-active,
.list-leave-active,
.list-move {
  transition: 1s cubic-bezier(0.59, 0.12, 0.34, 0.95);
  transition-property: opacity, transform;
}

.list-enter {
  opacity: 0;
  transform: translateX(50px) scaleY(0.6);
}

.list-enter-to {
  opacity: 1;
  transform: translateX(0) scaleY(1);
}

.list-leave-active {
  position: absolute;
}

.list-leave-to {
  opacity: 0;
  transform: scaleY(0);
  transform-origin: center top;
}
</style>
