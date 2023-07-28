<template>
    <div class="container" :style="`background: url(${image}); ${size};`">
        <span class="loader" v-if="!showDetails"></span>

        <ul v-if="showDetails">
            <button @click="addToFav"><i class="fa-regular fa-heart"></i></button>
            <div>
                <li class="temp-icon">{{ weather.temp }} °</li>
                <img class="temp-icon" :src="`${iconRender}`" alt="">
            </div>
            <div class="group">
                <div class="city-details">
                    <li class="city">{{ weather.name }}</li>
                    <li>Feels like {{ weather.feelsLike }} °</li>
                </div>
                <div class="city-details">
                    <div class="min-max">
                        <li>{{ weather.tempMax }} °</li>
                        <li> / {{ weather.tempMin }} °</li>
                    </div>
                    <li>{{ weather.desc }}</li>
                </div>
            </div>
        </ul>
    </div>
    <city-style :weather="weather" @background="changeBackground"></city-style>
</template>

<script>
import CityStyle from './CityStyle.vue'
export default {
    components: {
        CityStyle
    },
    emits: ['weather-details', 'add-main-fav'],
    props: ['city', 'newcity'],
    data() {
        return {
            key: "488b04013570707b21e11a9d0b01ebd1",
            weather: '',
            weatherDetails: '',
            newIcon: '',
            cityToCall: '',
            showDetails: false,
            image: '',
            weatherToPass: ''
        }
    },
    methods: {
        async getCityInfo() {
            try {
                const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.cityToCall}&appid=${this.key}&units=imperial`);
                if (!response.ok) {
                    this.cityToCall = this.weather.name
                    alert('Please add a valid City name')
                    return;
                } else {
                    const data = await response.json();
                    this.weather = {
                        name: data.name,
                        temp: Math.round(data.main.temp),
                        tempMax: Math.round(data.main.temp_max),
                        tempMin: Math.round(data.main.temp_min),
                        feelsLike: Math.round(data.main.feels_like),
                        desc: data.weather[0].main,
                        icon: data.weather[0].icon
                    }
                    this.weatherDetails = data
                    this.$emit('weather-details', this.weatherDetails)
                    localStorage.setItem('city', this.cityToCall)
                    this.showDetails = true
                }
            } catch (error) {
                alert(error)
            }
        },
        addToFav() {
            this.$emit('add-main-fav', this.cityToCall)
        },
        changeBackground(value) {
            this.image = value
        }
    },
    computed: {
        iconRender() {
            return `http://openweathermap.org/img/wn/${this.newIcon}@2x.png`
        },
        size() {
            return 'background-size: cover; background-opacity: .2'
        },
    },
    watch: {
        city(val) {
            if (val) {
                this.cityToCall = val
                this.getCityInfo()
            }
        },
        weather(one) {
            this.newIcon = one.icon
        },
        newcity(name) {
            if (name) {
                this.cityToCall = name
                this.getCityInfo()
            }
        },
    },
    mounted() {
        const getCityFromLocalStorage = localStorage.getItem('city')
        this.cityToCall = getCityFromLocalStorage
        if (this.cityToCall) {
            this.getCityInfo()
        } else {
            this.cityToCall = 'Fort Lauderdale'
            this.getCityInfo()
        }

        
    }
}
</script>

<style>
.container {
    background-size: cover;
    width: 100%;
    border-radius: 36px;
    box-shadow: 1px 1px 8px rgba(0, 0, 0, 0.2);
    margin: 10px 0;
    position: relative;
}

ul {
    padding: 0;
}

.loader {
    top: 200px;
    color: #fff;
    font-size: 10px;
    width: 1em;
    height: 1em;
    border-radius: 50%;
    position: absolute;
    text-indent: -9999em;
    animation: mulShdSpin 1.3s infinite linear;
    transform: translateZ(0);
}

@keyframes mulShdSpin {

    0%,
    100% {
        box-shadow: 0 -3em 0 0.2em,
            2em -2em 0 0em, 3em 0 0 -1em,
            2em 2em 0 -1em, 0 3em 0 -1em,
            -2em 2em 0 -1em, -3em 0 0 -1em,
            -2em -2em 0 0;
    }

    12.5% {
        box-shadow: 0 -3em 0 0, 2em -2em 0 0.2em,
            3em 0 0 0, 2em 2em 0 -1em, 0 3em 0 -1em,
            -2em 2em 0 -1em, -3em 0 0 -1em,
            -2em -2em 0 -1em;
    }

    25% {
        box-shadow: 0 -3em 0 -0.5em,
            2em -2em 0 0, 3em 0 0 0.2em,
            2em 2em 0 0, 0 3em 0 -1em,
            -2em 2em 0 -1em, -3em 0 0 -1em,
            -2em -2em 0 -1em;
    }

    37.5% {
        box-shadow: 0 -3em 0 -1em, 2em -2em 0 -1em,
            3em 0em 0 0, 2em 2em 0 0.2em, 0 3em 0 0em,
            -2em 2em 0 -1em, -3em 0em 0 -1em, -2em -2em 0 -1em;
    }

    50% {
        box-shadow: 0 -3em 0 -1em, 2em -2em 0 -1em,
            3em 0 0 -1em, 2em 2em 0 0em, 0 3em 0 0.2em,
            -2em 2em 0 0, -3em 0em 0 -1em, -2em -2em 0 -1em;
    }

    62.5% {
        box-shadow: 0 -3em 0 -1em, 2em -2em 0 -1em,
            3em 0 0 -1em, 2em 2em 0 -1em, 0 3em 0 0,
            -2em 2em 0 0.2em, -3em 0 0 0, -2em -2em 0 -1em;
    }

    75% {
        box-shadow: 0em -3em 0 -1em, 2em -2em 0 -1em,
            3em 0em 0 -1em, 2em 2em 0 -1em, 0 3em 0 -1em,
            -2em 2em 0 0, -3em 0em 0 0.2em, -2em -2em 0 0;
    }

    87.5% {
        box-shadow: 0em -3em 0 0, 2em -2em 0 -1em,
            3em 0 0 -1em, 2em 2em 0 -1em, 0 3em 0 -1em,
            -2em 2em 0 0, -3em 0em 0 0, -2em -2em 0 0.2em;
    }
}
</style>
<style scoped>
div {
    display: flex;
    justify-content: center;
}

.temp-icon {
    width: 100px;
    height: 100px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 36px;
    font-weight: bold;
    margin: 0 10px 10px 0;
}

.city-details {
    display: flex;
    justify-content: space-between;
    flex-direction: column;
}

button {
    position: absolute;
    top: 0;
    right: 0;
    padding: 15px 25px;
    border-radius: 0px 35px 0px 35px;
    background: none;
    border: none;
    font-size: 16px;
    border: 1px solid rgba(0, 0, 0, 0.3);
    background-color: rgba(137, 43, 226, 0.1);
    opacity: .8;
}

li {
    margin: 5px 0;
}

i {
    color: rgb(0, 0, 0);
}

.city {
    font-weight: bold;
}

.group {
    display: flex;
    justify-content: space-between;
}
</style>