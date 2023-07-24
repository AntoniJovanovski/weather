<template>
    <div>
        <form @submit.prevent="cityInput">
            <input type="text" v-model="input" placeholder="Search For a City">
            <button @click="cityInput"><i class="fa-solid fa-magnifying-glass"></i></button>
        </form>
        <form @submit.prevent="addFavCity">
            <input type="text" v-model="favouriteCity" placeholder="Add Favourite Cities">
            <button @click="addFavCity"><i class="fa-solid fa-plus"></i></button>
        </form>
    </div>
</template>

<script>
export default {
    emits: ['input', 'fav-city'],
    data() {
        return {
            input: '',
            favouriteCity: ''
        }
    },
    methods: {
        addFavCity() {
            this.$emit('fav-city', this.favouriteCity)
            this.favouriteCity = ''
        },
        cityInput() {
            this.$emit('input', this.input)
            this.input = ''
        }
    }
}
</script>

<style scoped>
input {
    width: 150px;
    height: 25px;
    text-align: center;
    border-radius: 50px;
    border: none;
    box-shadow: 1px 1px 3px rgba(0, 0, 0, 0.1);
    background-color: rgba(255, 255, 255, 0.2);
}
input::placeholder {
    color: rgba(37, 37, 37, 0.5);
}
button {
    width: 25px;
    height: 25px;
    background: none;
    border: none;
}
div {
    display: flex;
    justify-content: center;
    padding: 10px 0;
}
form {
    margin: 0 10px;
}
i {
    color: black;
}
@media only screen and (max-width: 600px) {
    input {
        width: 120px;
    }
}
</style>