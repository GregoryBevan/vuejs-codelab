<template>
    <div>
        <h1>Liste des séries</h1> 
        <div id="search">
            <input type="text" v-model="search" class="form-control" placeholder="Filtrer...">
        </div>
        <ul>
            <serie v-for="serie in filteredSeries" :key="serie.id" :serie-details="serie" @clicked="toggleFavorites($event)"></serie>
        </ul>
    </div>
</template>

<script>
import Serie from '@/components/Serie.vue'
import seriesService from '@/services/series.service'
import favoritesService from '@/services/favorites.service'

export default {
    components: {
        Serie
    },
    data () { 
        return { 
            series: [],
            search: ''
        } 
    }, 
    mounted () {
        seriesService.getSeries().then(response => (this.series = response.map(item => item.show)))
    },
    methods: {
        toggleFavorites (serie) {
            favoritesService.isFavorite(serie) ? favoritesService.removeFavorite(serie) : favoritesService.addFavorite(serie) 
        }
    },
    computed: {
        filteredSeries () {
                  return this.search === '' ? this.series : this.series.filter(serie => serie.name.toLowerCase().includes(this.search.toLowerCase()))
        }
    }
}
</script>

<style scoped>
#search {
    padding: 10px 50px 0px 50px;
}
ul {
    -webkit-padding-start: 0px;
    padding: 50px;
}
</style>
