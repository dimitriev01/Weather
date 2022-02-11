<template>
         <div class="welcome">
             Погода сейчас! Дата : {{ this.timestamp }}
         </div>
         <div class="search__box">
                <input
                    type="text" 
                    class="search__box-search__bar" 
                    placeholder="Введите город" 
                    v-model="query"
                    @keypress="getWeather"
                />
         </div>
        
        <transition name="weather">
            <div key="this.key" class="weather-weather__wrap" v-if="typeof weather.main != 'undefined'">
                <div class="weather-weather__wrap-location__box">
                    <div class="weather-weather__wrap-location__box-location">{{ weather.name }}, {{ weather.sys.country }}</div>
                </div>
                <div class="weather-weather__wrap-weather__box">
                    <div class="weather-weather__wrap-weather__box-weather">
                        {{ Math.round((weather.main.temp - 32) / 1.8) }}°C
                        {{ weather.weather[0].description }}
                        <img class="weather-weather__wrap-weather__box-weather-icon" :src="getIcon()">
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

.search__box-search__bar {
    min-width: 100px;
}

.search__box {
    border: 0;
    width: 40%;
    display: flex;
    margin: 0 auto;
    margin-bottom: 30px;
    flex-direction: column;
    align-items: center;
}

.search__box .search__box-search__bar{
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

.weather-weather__wrap-location__box .weather-weather__wrap-location__box-location {
    color: white;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px 3px rgba(0, 0, 0, 0.5);
}

.weather-weather__wrap-weather__box {
    text-align: center;
}

.weather-weather__wrap-weather__box .weather-weather__wrap-weather__box-weather {
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

@media (max-width: 407px) {
    .weather-weather__wrap-weather__box .weather-weather__wrap-weather__box-weather {
        padding: 5px 15px;
    }
}

@media (max-width: 750px) {
    .weather-weather__wrap-weather__box .weather-weather__wrap-weather__box-weather {
        font-size: 30px;
    }
}

</style>