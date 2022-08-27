<template>
    <div class="container-fluid">
        <div class="row data__container">
            <div class="col-12 col-lg-4 form__container">
                <div class="inner__form__container">
                    <h1>دليل الهيئة</h1>
                    <form @submit.prevent="fetchPlacesForCity()">
                        <label>
                            الدول
                            <select v-model="selectedCountry">
                                <option
                                    value=""
                                    selected
                                    disabled
                                    hidden
                                >
                                    اختار الدوله
                                </option>
                                <option
                                    v-for="country in countries"
                                    :key="country"
                                >
                                    {{ country }}
                                </option>
                            </select>
                            <p
                                v-if="
                                    v$.selectedCountry.required.$invalid &&
                                    isSubmitted
                                "
                                class="error"
                            >
                                برجاء اختيار الدوله
                            </p>
                        </label>
                        <label>
                            المدن
                            <select v-model="selectedCity">
                                <option
                                    value=""
                                    selected
                                    disabled
                                    hidden
                                >
                                    اختار المدينه
                                </option>
                                <option
                                    v-for="state in getCountryStates"
                                    :key="state"
                                >
                                    {{ state }}
                                </option>
                            </select>
                            <p
                                v-if="
                                    v$.selectedCity.required.$invalid &&
                                    isSubmitted
                                "
                                class="error"
                            >
                                برجاء اختيار المدينه
                            </p>
                        </label>
                        <button>
                            اعرض النتائج
                            <img src="./assets/arrow.svg" />
                        </button>
                    </form>
                </div>
            </div>

            <div class="col-12 col-lg-8 map__container">
                <Map :locations="cityPlaces" />
            </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
import Map from './components/Map.vue';
import useVuelidate from '@vuelidate/core';
import { required } from '@vuelidate/validators';

export default {
    name: 'App',
    setup() {
        return { v$: useVuelidate() };
    },
    data() {
        return {
            countries: [],
            selectedCountry: '',
            selectedCity: '',
            countryStates: {},
            cityPlaces: [[0, 0]],
            isSubmitted: false,
        };
    },
    validations() {
        return {
            selectedCountry: {
                required,
            },
            selectedCity: {
                required,
            },
        };
    },
    components: {
        Map,
    },
    methods: {
        fetchCountryList() {
            axios(
                'https://raw.githubusercontent.com/russ666/all-countries-and-cities-json/master/countries.min.json'
            ).then((data) => {
                this.countries = Object.keys(data.data);
                this.countryStates = data.data;
            });
        },
        fetchPlacesForCity() {
            this.isSubmitted = true;
            if (this.selectedCity && this.selectedCountry) {
                axios(
                    `https://nominatim.openstreetmap.org/search?q=${this.selectedCity}&format=json`
                ).then((data) => {
                    console.log(data.data);
                    let newDataMap = data.data.map((d) => {
                        return [d.lat, d.lon];
                    });
                    this.cityPlaces = newDataMap;
                });
            }
        },
    },
    mounted() {
        this.fetchCountryList();
    },
    computed: {
        getCountryStates() {
            return this.countryStates[this.selectedCountry];
        },
    },
};
</script>
<style scoped lang="scss">
@import './styles/main.scss';
@import './styles/form.scss';
</style>
