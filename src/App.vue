<template>
 <div id="app">
     <main :class="is_day ? 'day' : 'night'">
         <div class="welcome">
             Погода сейчас! {{ dateBuilderAccurate() }}
         </div>
         <div class="search-box">
                <input
                    type="text" 
                    class="search-bar" 
                    placeholder="Введите город и нажмите Enter..." 
                    v-model="query"
                    @keypress="getWeather"
                />
         </div>
         <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
             <div class="location-box">
                 <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
             </div>
             <div class="weather-box">
                 <div class="temp">{{ Math.round((weather.main.temp - 32) / 1.8) }}°C</div>
                 <div class="weather">
                     {{ weather.weather[0].description }}
                     <img class="icon" :src="getIcon()">
                 </div>
             </div>
         </div>
     </main>
 </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'App',
    data(){
        return	{
            api_key_weather: `0aa7f8ed193e60c3c4080972bb970326`,
            url_base_weather: "https://api.openweathermap.org/data/2.5/",
            query: '',
            is_day: '',
            weather: {},
            icon: '',
        }
    },
    methods: {
        getWeather (e) {
            if (e.key == "Enter" && this.query) {
                
            axios.get(`${this.url_base_weather}weather?q=${this.query}&lang=ru&units=imperial&appid=${this.api_key_weather}`)
                .then(res => {
                    return res.data;
                }).then(this.setWeatherResults);
                this.query = '';
            } else {
                if (e.key == "Enter") 
                    alert('Введите город!')
            }
        },
        setWeatherResults (results) {
            this.weather = results;
            this.checkTime();
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
            return `${year}, ${date} ${month}, ${hours}:${minutes}:${sec}`;
        },
        checkTime(){
            let timeOfDay = this.weather.weather[0].icon;

            if (timeOfDay.includes('n')){
                this.is_day = false;
            } else {
                this.is_day = true;
            }
        },
        getIcon(){
           this.icon = this.weather.weather[0].icon;
            return `http://openweathermap.org/img/wn/${this.icon}.png`; 
        },
    }
}
</script>
