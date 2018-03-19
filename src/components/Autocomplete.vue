<template>
    <section>
        <!-- On associe la variable search du data à l'input
        et on rajoute un event input qui déclenchera la méthode complete -->
        <input type="text" v-model="search"
             @input="complete()" >
             <!-- Si on a des villes renvoyées par l'API -->
        <ul v-if="cities.length > 0" class="suggestions">
            <!-- On boucle dessus pour les afficher -->
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
            // On fait un appel ajax vers l'api geonames en concatenant
            //la valeur de la variable search dans l'url de l'api
            axios.get(`http://api.geonames.org/postalCodeSearchJSON?placename_startsWith=${this.search}&country=FR&maxRows=10&username=simplonlyon`)
            //quand on a une réponse, on la met dans notre variable cities
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
