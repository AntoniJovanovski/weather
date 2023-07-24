<template>
    <div class="local-container">

        <ul class="container" v-for="(data, index) in data" :key="data.name" @click="viewCityDetails(data.name)"
            :style="`background: url(${data.picture[index]});  background-size: cover;`">

            <div>
                <li class="name">{{ data.name }}
                    <i class="fa-regular fa-heart"></i>
                </li>

                <li>{{ Math.round(data.temp) }} °</li>
            </div>

            <li class="image">
                <img :src="`http://openweathermap.org/img/wn/${data.icon}@2x.png`" alt="icon">
            </li>
            <button @click.stop="removeCity(index)">
                <i class="fa-regular fa-trash-can"></i></button>

        </ul>
    </div>
    <city-style :tempFromFav="cityToPass" @background="changeBG"></city-style>
</template>

<script>
import { onUpdated } from 'vue';
import CityStyle from './CityStyle.vue';
export default {
    props: ['favourite', 'fav',],
    emits: ['view-city', 'delete'],
    components: {
        CityStyle
    },
    data() {
        return {
            key: "488b04013570707b21e11a9d0b01ebd1",
            data: [],
            cityToPass: '',
            arrayBG: [],
        }
    },
    methods: {
        async getFavouriteCity(city) {
            try {
                const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${this.key}&units=imperial`);
                if (!response.ok) {
                    alert('Please add valid city name')
                } else {
                    const data = await response.json();
                    this.cityToPass = data.main.temp
                    this.data.unshift({
                        name: data.name,
                        temp: data.main.temp,
                        icon: data.weather[0].icon,
                        picture: this.arrayBG
                    }),
                        localStorage.setItem('Favourites', JSON.stringify(this.data))
                }
            } catch (error) {
                alert(error)
            }
        },
        removeCity(idx) {
            this.data.splice(idx, 1)
            this.arrayBG.splice(idx, 1)
            localStorage.setItem('Favourites', JSON.stringify(this.data))
            this.$emit('delete')
            this.cityToPass = ''
        },
        viewCityDetails(cityName) {
            this.$emit('view-city', cityName)
            window.scrollTo({
                top: 0,
                behavior: 'smooth'
            });
        },
        changeBG(value) {
            this.arrayBG.unshift(value)
        },
        callOnMount() {
            if (localStorage.getItem('Favourites')) {
                const getCities = localStorage.getItem('Favourites')
                const cities = JSON.parse(getCities)
                for (const city of cities) {
                    this.getFavouriteCity(city.name)
                }
            }
        }
    },
    watch: {
        favourite(fav) {
            if (fav) {
                if (this.data.some(city => city.name === fav) || this.data.some(city => city.name.toLowerCase() === fav)) {
                    alert(`${fav} is already added to favourites`);
                    return;
                } else {
                    this.getFavouriteCity(fav);
                }
            }
        },
        fav(name) {
            if (name) {
                if (this.data.some(city => city.name === name) || this.data.some(city => city.name.toLowerCase() === name)) {
                    alert(`${name} is already added to favourites`);
                    return;
                } else {
                    this.getFavouriteCity(name);
                }
            }
        },

    },
    mounted() {
        this.callOnMount()
    },
}
</script>

<style scoped>
/* width */
::-webkit-scrollbar {
    width: 5px;
}

/* Track */
::-webkit-scrollbar-track {
    background: none;

}

/* Handle */
::-webkit-scrollbar-thumb {
    background: #88888893;
    border-radius: 5px;

}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
    background: #555;
}

.local-container {
    margin-top: 50px;
    height: 530px;
    width: auto;
    overflow: auto;
}

.container {
    position: relative;
    margin: 10px;
    width: auto;
}

.container:hover {
    cursor: pointer;
}

.container:active {
    scale: .985;
}

.image {
    text-align: center;
}

.container div {
    display: flex;
    justify-content: space-between;
    margin-top: 10px;
    padding: 20px 80px;
}

button {
    position: absolute;
    left: 0;
    bottom: 0;
    padding: 15px 25px;
    border-radius: 0px 35px 0px 35px;
    background: none;
    border: none;
    font-size: 16px;
    border: 1px solid rgba(0, 0, 0, 0.3);
    background-color: rgba(137, 43, 226, 0.07);
    opacity: .8;
}

.name {
    font-weight: bold;
    font-size: 18px;
    margin-right: 10px;
}

@media only screen and (max-width: 700px) {
    .local-container {
        margin-top: 50px;
        width: auto;
        height: auto;
        overflow: auto;
    }
}
</style>