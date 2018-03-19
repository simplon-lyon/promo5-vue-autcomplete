<template>
    <section>
        <input type="text" v-model="search"
             @input="complete()" >
        <ul v-if="cities.length > 0" class="suggestions">
            <li v-for="(city,index) in cities" :key="index">
                {{city.placeName}}, {{city.postalCode}}
            </li>
        </ul>
    </section>
</template>

<script>
import axios from 'axios'

export default {
    name:'autocomplete',
    data() {
        return {
            search: '',
            cities: []
        }
    },
    methods: {
        complete() {
            axios.get(`http://api.geonames.org/postalCodeSearchJSON?placename_startsWith=${this.search}&country=FR&maxRows=10&username=simplonlyon`)
            .then((response) => this.cities = response.data.postalCodes)
            .catch( (error) => console.error(error));
        }
    }
}
</script>

<style>
.suggestions {
    list-style-type: none;
    border: 1px solid black;
    border-top: none;
    width: 167px;
    margin-left: 234px;
    margin-top: 0;
    padding: 2px;
}

.suggestions>li:hover {
    background-color: #a7eeea;
}

</style>
