<template>
 <div id="app">
     <main :class="checkBackgroud()">
         <div class="welcome">
             Погода сейчас! {{ dateBuilderAccurate() }}
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
            <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
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

         <div class="nasa">
            <h3 class="h3-nasa">
                Данные, полученные по API NASA 
            </h3>
            <button 
                class="btn-nasa"
                @click="getNasa"
            >
                Получить данные по API NASA 
            </button>
            <div class="elems-nasa">
                  <div :class="visible" class="elem-nasa">{{ this.nasa.title }} </div>
                  <p :class="visible" class="elem-nasa"><img :src="this.nasa.url" alt=""></p>
                  <p :class="visible" class="elem-nasa"> {{ this.nasa.explanation }} </p>
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
            api_key_nasa : 'gXj5IgX5hRZpDpIFtPreJpkAXhbFV7trJs6K2F0J',
            url_base_nasa : 'https://api.nasa.gov/planetary/apod?api_key=', 
            url_base_weather: "https://api.openweathermap.org/data/2.5/",
            query: '',
            is_day: '',
            weather: {},
            defaultBackgroud: true,
            nasa: {},
            icon: '',
            visible: 'false',
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
        getNasa () {
            axios.get(`${this.url_base_nasa}${this.api_key_nasa}`)
            .then(res => {
                return res.data;
            }).then(this.setNasaResults);
        },
        setWeatherResults (results) {
            this.weather = results;
            this.checkTime();
        },
        setNasaResults (results) {
            this.visible = true;
            this.nasa = results;
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
                this.defaultBackgroud = false;
            } else {
                this.is_day = true;
                this.defaultBackgroud = false;
            }
        },
        getIcon(){
            this.icon = this.weather.weather[0].icon;
            return `http://openweathermap.org/img/wn/${this.icon}.png`; 
        },
        checkBackgroud(){
           if (!this.defaultBackgroud){
               if (this.is_day) return 'day';
               else return 'night';
           }
        },
    }
}
</script>
