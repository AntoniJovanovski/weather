<template>
    <div class="container">
        <ul v-if="show">
            <div class="details">
                <li 
                v-for="text in renderData.list" 
                :key="text"
                > {{ text.dt_txt.slice(10, 16) }}</li>
            </div>

            <div class="details">
                <li 
                v-for="temp in renderData.list" 
                :key="temp"
                > {{ Math.round(temp.main.temp) }} °</li>
            </div>

            <div class="pictures">
                <li 
                v-for="data in renderData.list">
                <img
                :src="`http://openweathermap.org/img/wn/${data.weather[0].icon}@2x.png`" 
                alt="icon"></li>
            </div>
        </ul>
    </div>
</template>

<script>
export default {
    props: ['city', 'newcity'],
    data() {
        return {
            key: "488b04013570707b21e11a9d0b01ebd1",
            renderData: '',
            cityToCall: '',
            show: false
        }
    },
    methods: {
        async hourlyData() {
            try {
                const response = await fetch(`https://api.openweathermap.org/data/2.5/forecast?q=${this.cityToCall}&appid=${this.key}&units=imperial&cnt=9`);
                if (!response.ok) {
                    return;
                } else {
                    const data = await response.json();
                    this.renderData = data
                    localStorage.setItem('city', this.cityToCall)
                    this.show = true
                }
            } catch (error) {
                alert(error)
            }
        }
    },
    watch: {
        city(value) {
            if (value) {
                this.cityToCall = value
                this.hourlyData()
            }
        },
        newcity(name) {
            this.cityToCall = name
            this.hourlyData()
        }
    },
    mounted() {
        if (localStorage.getItem('city')) {
            const call = localStorage.getItem('city')
            this.cityToCall = call
            this.hourlyData()
        } else {
            this.cityToCall = 'Fort Lauderdale'
            this.hourlyData()
        }
    }
}

</script>

<style scoped>
.details {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    font-size: 12px;
    margin: 8px 14px
}
.pictures {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    font-size: 8px;
    padding: 4px;
}
img {
    width: 35px;
    height: 35px;
}
ul {
    padding: 10px 5px;
}
</style>
