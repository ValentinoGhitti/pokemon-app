<template>
    <div class="wrapper">
        <div class="device">
            <div class="btn-volume">
            <div class="btn-volume-up"></div>
            <div class="btn-volume-down"></div>
        </div>
        <div class="btn-power">
            <div class="btn-power-act"></div>
        </div>
        <div class="header">
            <div class="speaker"></div>
            <div class="detector detector-1"></div>
            <div class="detector detector-2"></div>
            <div class="camera"></div>
        </div>
        <div class="display">
            <h1 v-if="!pokemon"></h1>
            <div v-else>
                <h1>¿Quién es ese pokemon?</h1>
                <PokemonPicture 
                :pokemonId="pokemon.id" 
                :showPokemon="showPokemon" 
                />
                <PokemonOptions 
                :pokemons="pokemonArr"
                @selection="checkAnswer"
                />
                <div v-if="showPokemon">
                    <h2 class="fade-in">{{message}}</h2>
                </div>
            </div>
            <div class="myClass" id="myDiv"></div>
        </div>
    </div>
</div>


</template>

<script>
import PokemonOptions from '../components/PokemonOptions.vue'
import PokemonPicture from '../components/PokemonPicture.vue'
import getPokemonOptions from '../helpers/getPokemonOptions'


export default {
    name: 'PokemonPage',
    components: {
        PokemonOptions,
        PokemonPicture
    },
    data() {
        return {
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: ''
        }
    },
    methods: {
        async mixPokemonArray() {
            this.pokemonArr = await getPokemonOptions()
            const rnd = Math.floor( Math.random() * 4)
            this.pokemon = this.pokemonArr[rnd]
        },
        checkAnswer(pokemonId) {
            this.showPokemon = true
            this.showAnswer = true
            if (pokemonId === this.pokemon.id ) {
                this.pokemonArr = false
                this.$swal.fire({
                    toast: true,
                    title: `Meow, así es. 
                    ¡Es ${this.pokemon.name.toUpperCase()}!`,
                    icon: 'success',
                    position: 'bottom',
                    width: '300px',
                    target: document.getElementById('myDiv'),
                    showConfirmButton: false,
                    timer: 3000,
                    timerProgressBar: true,
                })
                .then((willDelete) => {
                if(willDelete) {
                    this.newGame()
                }
            })
            } else {
                this.pokemonArr = false
                this.$swal.fire({
                    toast: true,
                    title: `¡No!, era ${this.pokemon.name.toUpperCase()}`,
                    icon: 'error',
                    position: 'bottom',
                    width: '300px',
                    target: document.getElementById('myDiv'),
                    showConfirmButton: false,
                    timer: 3000,
                    timerProgressBar: true,
                })
                .then((willDelete) => {
                    if(willDelete) {
                        this.newGame()
                    }
                }) 
            }
        },
        newGame() {
            this.wrongOption = false 
            this.correctOption = false
            this.showPokemon = false,
            this.pokemonAnswer = false,
            this.pokemonArr = [],
            this.pokemon = null
            this.mixPokemonArray()
        },
    },
    mounted() {
        this.mixPokemonArray() 
    }
}

</script>