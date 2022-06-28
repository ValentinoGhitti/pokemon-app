<template>
    <div class="options-container">
        <ul>
            <li v-for="pokemon in pokemons" 
            :key="pokemon.id"
            @click="$emit('selection', pokemon.id )"
            >
            {{ pokemon.name.toUpperCase() }}
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    name: 'PokemonOptions',
    props: {
        pokemons: {
            typeof: Array,
            required: true,
        }
    },
}
</script>

<style scoped>
ul {
    list-style-type: none;
}
li {
    background-color: white;
    border-radius: 5px;
    border: 3px solid rgba(0, 0, 0, 0.2);
    cursor: pointer;
    margin-bottom: 15px;
    padding: 5px;
    width: 260px;
    margin-right: 30px;
}

li:hover {
    background-color: rgba(0, 0, 0, 0.05);
}

.options-container {
    display: flex;
    justify-content: center;
}
</style>