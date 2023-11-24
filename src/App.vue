<script>
import { ref } from 'vue';
import AppHeader from './components/AppHeader.vue';
import AppFooter from './components/AppFooter.vue';

export default {
  components: { AppHeader, AppFooter },
  setup() {
    const loading = ref(true);
    const houses = ref([]);

    const filters = ref({
      cities: [],
      priceMin: undefined,
      priceMax: undefined,
      bedrooms: undefined,
      hometypeid: undefined,
    });

    const filterData = ref({
      cities: [],
      homeTypes: [],
    });

    const getData = function (url) {
      return fetch(url).then(function (response) {
        return response.json();
      });
    };

    const getHouses = async function () {
      const { data } = await getData(`https://realestate-api.fgmnts.be/api/v1/homes/?nopagination=true&page=1`);
      houses.value = data;
      loading.value = false;
    };

    const fillFilter = async function () {
      const cities = await getData(`https://realestate-api.fgmnts.be/api/v1/cities`);
      filterData.value.cities = cities.data;
      const homeTypes = await getData(`https://realestate-api.fgmnts.be/api/v1/hometypes`);
      filterData.value.homeTypes = homeTypes.data;
    };

    const filterHouses = function () {
      console.log(filters.value);
    };

    getHouses();
    fillFilter();

    console.log(houses.value);
    return { loading, houses, filterData, filters, filterHouses };
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

    <!-- ======= Search ======= -->
    <section>
      <div class="container mb-4">
        <div class="row">
          <div class="col-lg-3 col-md-6 mb-2">
            <div class="form-group mt-3">
              <label class="form-label" for="gemeente">Gemeentes</label>
              <select class="form-control" id="gemeente" name="city" multiple v-model="filters.city" @change="filterHouses">
                <option :value="undefined">Maak een keuze...</option>
                <option v-for="city in filterData.cities" :value="city.city">{{ city.city }} ({{ city.number_of_homes }})</option>
              </select>
            </div>
          </div>
          <div class="col-lg-3 col-md-6 mb-2">
            <div class="form-group mt-3">
              <label class="form-label" for="type">Type</label>
              <select class="form-control" id="type" name="type">
                <option :value="undefined">Maak een keuze...</option>
                <option v-for="homeType in filterData.homeTypes" :value="homeType.id">{{ homeType.description }}</option>
              </select>
            </div>
          </div>
          <div class="col-lg-3 col-md-6 mb-2 d-flex">
            <div class="c-input form-group mt-3 me-1">
              <label class="form-label" for="prijsmin">Prijs min.</label>
              <input class="form-control" type="number" step="50000" id="prijsmin" name="pricemin" />
            </div>
            <div class="c-input form-group mt-3 ms-1">
              <label class="form-label" for="prijsmax">max.</label>
              <input class="form-control" type="number" step="50000" id="prijsmax" name="pricemax" />
            </div>
          </div>
          <div class="col-lg-3 col-md-6 mb-2">
            <div class="form-group mt-3">
              <label class="form-label" for="opp">Slaapkamers min.</label>
              <select class="form-control" id="opp" name="bedrooms">
                <option :value="undefined">Maak een keuze...</option>
                <option value="1">1</option>
                <option value="2">2</option>
                <option value="3">3</option>
                <option value="4">4</option>
              </select>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- End Search -->

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
                        <span>{{ house.squarefootage }}<sup>2</sup></span>
                      </li>
                      <li>
                        <h4 class="c-pand__label">Slpks</h4>
                        <span>{{ house.bedrooms }}</span>
                      </li>
                      <li>
                        <h4 class="c-pand__label">Badk</h4>
                        <span>{{ house.bathrooms }}</span>
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
