<template>
    <div class="container">
        <ul v-if="showDetails">
            <div class="details">
                <div>
                    <li class="name">Wind </li>
                    <i class="fa-solid fa-wind"></i>
                    <li class="text">{{ data.wind }} mp/h</li>
                </div>
                <div>
                    <li class="name">Humidity </li>
                    <i class="fa-solid fa-droplet"></i>
                    <li class="text"> {{ data.humidity }}%</li>
                </div>
            </div>

            <div class="details">
                <div>
                    <li class="name">Sunrise </li>
                    <i class="fa-regular fa-sun"></i>
                    <li class="text">{{ data.sunrise }} am</li>
                </div>
                <div>
                    <li class="name">Sunset </li>
                    <i class="fa-solid fa-sun"></i>
                    <li class="text">{{ data.sunset }} pm</li>
                </div>
            </div>
        </ul>
    </div>
</template>

<script>
export default {
    props: ['details'],
    data() {
        return {
            data: {},
            showDetails: false
        }
    },
    methods: {
        convert() {
            // CONVERT SUNRISE
            let sunrise = new Date(this.data.sunrise * 1000);
            let SRtime = sunrise.toLocaleString().slice(10, -6)
            this.data.sunrise = SRtime
            // CONVERT SUNSET
            let sunset = new Date(this.data.sunset * 1000);
            let SStime = sunset.toLocaleString().slice(10, -6)
            this.data.sunset = SStime
        }
    },
    watch: {
        details(val) {
            this.data = {
                wind: val.wind.speed,
                humidity: val.main.humidity,
                sunrise: val.sys.sunrise,
                sunset: val.sys.sunset
            }
            this.showDetails = true
            this.convert()
        }
    }
}
</script>

<style scoped>
.text {
    font-size: 12px;
}
li {
    margin: 5px 0;
}
.details {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin: 0 40px;
    align-items: center;
}
.details div {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin: 10px 40px;
    width: 100%;
}
.name {
    font-weight: bold;
    font-size: 14px;
}
</style>