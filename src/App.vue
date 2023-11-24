<script>
import { ref } from 'vue';
import AppHeader from './components/AppHeader.vue';
import AppFooter from './components/AppFooter.vue';

export default {
  components: { AppHeader, AppFooter },
  setup() {
    const loading = ref(true);
    const houses = ref([]);

    const getHouses = async function () {
      const jsonData = await fetch(`https://realestate-api.fgmnts.be/api/v1/homes/?nopagination=true&page=1`).then(function (response) {
        return response.json();
      });
      houses.value = jsonData.data;
      loading.value = false;
    };

    getHouses();
    console.log(houses.value);
    return { loading, houses };
  },
};
</script>

<template>
  <AppHeader />

  <main>
    <!-- ======= Intro ======= -->
    <section class="c-intro">
      <div class="container">
        <div class="row">
          <div class="col-12">
            <div class="c-intro__border">
              <h1 class="c-intro__title">Panden te koop</h1>
              <span class="u-color-text">De Panne en omgeving</span>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- End Intro -->

    <!-- ======= Property Grid ======= -->
    <section>
      <div class="container">
        <div class="row">
          <div class="c-loader" v-if="loading">
            <div class="c-loader__bar"></div>
            <div class="c-loader__bar"></div>
            <div class="c-loader__bar"></div>
          </div>
          <!-- Start voorbeeld: 1 pand -->
          <div class="col-md-4" v-for="house in houses">
            <div class="c-pand">
              <div class="c-pand__img"><img :src="house.photos[0]" alt="" class="c-pand__img img-fluid" /></div>
              <div class="c-pand__overlay">
                <div class="c-pand__content">
                  <div class="c-pand__header">
                    <h2 class="c-pand__title">{{ house.name }}</h2>
                    {{ house.adress }}<br />
                    {{ house.city }}
                  </div>
                  <div class="c-pand__body">
                    <div class="pb-1 d-flex">
                      <span class="c-pand__price">&euro; {{ house.price.toLocaleString() }}</span>
                    </div>
                  </div>
                  <div class="c-pand__footer">
                    <ul class="c-pand__info d-flex justify-content-around">
                      <li>
                        <h4 class="c-pand__label">Opp</h4>
                        <span>340m<sup>2</sup></span>
                      </li>
                      <li>
                        <h4 class="c-pand__label">Slpks</h4>
                        <span>2</span>
                      </li>
                      <li>
                        <h4 class="c-pand__label">Badk</h4>
                        <span>4</span>
                      </li>
                      <li>
                        <h4 class="c-pand__label">Garage</h4>
                        <span>Ja</span>
                      </li>
                    </ul>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <!-- End voorbeeld: 1 pand -->
        </div>
      </div>
    </section>
    <!-- End Property Grid -->
  </main>

  <AppFooter />
</template>

<style></style>
