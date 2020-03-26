<template>
    <article>
        <section >
            <form class="" @submit.prevent="searchGIFs()">
                <section class="">
                    <h3>Localizador de GIFS</h3>
                    <input type="text" class=""  v-model="query">
                    <button type="submit" class="" id="searchBtn" @disabled="disabledBtn">
                        Busca el gif
                    </button>
                </section>
            </form>
            <section class="" id="app-viewer" v-show="results">
                <img :src="current_gif" v-show="current_gif" class="">
                <h3 v-show="results.length === 0 || results.length === undefined" class="">¡Ops! No hay coincidencias</h3>
                <h3 v-show="results.length > 0 && !current_gif">Seleciona un gif</h3>
            </section>
            <section v-show="results" class="">
                <img :src="gif.images.fixed_height_small.url" :alt="gif.title" class="" v-for="gif in results" :key="gif.id" @click="viewGIF(gif)">
            </section>
        </section>
    </article>
</template>

<script>
import axios from 'axios'
let url = 'https://api.giphy.com/v1/gifs/search';
let keyApi = 'HawWec7jTNhnd7bpJQ5FolSkLZodqa96';
export default {
    data: function() {
        return {
            query: '',
            results: false,
            current_gif: false,
            limit:0
        };
    },
    watch:{
        query(oldValue, newValue){
            if(oldValue !== newValue){
                this.limit =0
            }
        }
    },
    computed:{
        disabledBtn(){
            return (this.query.trim() == '' || this.limit >=20 || this.results.length < 10) //Cambiar limit a 100
        }
    },
    methods: {
        searchGIFs: function () {
            let self = this;
            axios.get(url, {
                params: {
                    api_key: keyApi,
                    q: self.query.split(' ').join('+'),
                    limit: self.limit +=1  //Cambiar a 10
                }
            })
            .then(function (res) {
                self.results = res.data.data;
                self.current_gif = false;
            })
            .catch(function (e) {
                console.log("Tienes este errorcito: " + e);
            });
        },
        // Modal
        viewGIF: function (gif) {
            console.log(gif)
            self.current_gif = gif.images.original.url;
        }
    }
}
</script>


<style scoped>

</style>
