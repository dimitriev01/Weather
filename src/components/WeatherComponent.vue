<template>
         <div class="welcome">
             Погода сейчас! Дата : {{ this.timestamp }}
         </div>
         <div class="search-box">
                <input
                    type="text" 
                    class="search-bar" 
                    placeholder="Введите город" 
                    v-model="query"
                    @keypress="getWeather"
                />
         </div>
        
        <transition name="weather">
            <div key="this.key" class="weather-wrap" v-if="typeof weather.main != 'undefined'">
                <div class="location-box">
                    <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
                </div>
                <div class="weather-box">
                    <div class="weather">
                        {{ Math.round((weather.main.temp - 32) / 1.8) }}°C
                        {{ weather.weather[0].description }}
                        <img class="icon" :src="getIcon()">
                    </div>  
                </div>
            </div>
        </transition>
</template>

<script>
    import axios from 'axios';
    
    export default {
        name: 'Weather',
        data(){
            return {
                api_key_weather: `0aa7f8ed193e60c3c4080972bb970326`,
                url_base_weather: "https://api.openweathermap.org/data/2.5/",
                query: '',
                icon: '',
                timestamp: '',
                defaultBackgroud: true,
                key: Date.now(),
                weather: {},
            }
        },
        mounted() {
            this.dateBuilderAccurate();
            setInterval(this.dateBuilderAccurate, 1000);
        },
        methods:{
            getWeather (e) {
                if (e.key == "Enter" && this.query) {
                    axios.get(`${this.url_base_weather}weather?q=${this.query}&lang=ru&units=imperial&appid=${this.api_key_weather}`)
                        .then(res => {
                            return res.data;
                        })
                        .then(this.setWeatherResults)
                        .catch(() => {
                            alert('Нет такого города')
                        });
                        this.query = '';
                    } else {
                        if (e.key == "Enter") 
                            alert('Введите город!')
                    }
                },
            dateBuilderAccurate () {
                let d = new Date();
                let months = ["Январь", "Февраль", "Март", "Апрель", "Май", "Июнь", "Июль", "Август", "Сентябрь", "Октябрь", "Ноябрь", "Декабрь"];
                let days = ["Вс", "Пн", "Вт", "Ср", "Чт", "Пт", "Сб", 'Вс'];
                let date = d.getDate();
                let hours = d.getHours();
                let month = months[d.getMonth()];
                let year = d.getFullYear() + 'г.';
                let minutes = d.getMinutes();
                let sec = d.getSeconds();
                this.timestamp =  `${year}, ${date} ${month}, ${hours}:${minutes}:${sec}`;
            },
            checkTime(){
                let timeOfDay = this.weather.weather[0].icon;

                if (timeOfDay.includes('n')){
                    this.is_day = false;
                    this.defaultBackgroud = false;
                } else {
                    this.is_day = true;
                    this.defaultBackgroud = false;
                }
                this.$emit('background', this.defaultBackgroud, this.is_day)
            },
            getIcon(){
                this.icon = this.weather.weather[0].icon;
                return `http://openweathermap.org/img/wn/${this.icon}.png`; 
            },
            setWeatherResults (results) {
                this.weather = results;
                this.checkTime();
            },
        },
    }
</script>

<style scoped>
.welcome {
    color: #ff9c9c;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.5);
    padding: 0 0 20px 0;
}

.search-bar {
    min-width: 100px;
}

.search-box {
    border: 0;
    width: 40%;
    display: flex;
    margin: 0 auto;
    margin-bottom: 30px;
    flex-direction: column;
    align-items: center;
}

.search-box .search-bar{
    text-align: center;
    padding: 15px;
    color: #300;
    font-size: 20px;
    border: none;
    outline: none;
    background: none;
    box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.5);
    transition: .4s;
}

.location-box .location {
    color: white;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.5);
}

.location-box .date {
    color: white;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
}

.weather-enter-active,
.weather-leave-active .elems-nasa-enter-active,
.elems-nasa-leave-active {
    transition: opacity .5s;
}

.weather-enter,
.weather-leave-to,
.elems-nasa-enter,
.elems-nasa-leave-to {
    opacity: 0;
}

.weather-box {
    text-align: center;
}

.weather-box .weather {
    display: flex;
    border-radius: 10px;
    margin: 30px 0px;
    padding: 10px 25px;
    justify-content: center;
    align-items: center;
    background-color: rgb(233 141 141);
    color: white;
    font-size: 48px;
    font-weight: 700;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

</style>