<template>
 <div id="app">
     <main :class="checkBackgroud()">
         <weather-component
            @background='getBackground'
         ></weather-component>
         <nasa-component

         ></nasa-component>
     </main>
 </div>
</template>

<script>
import WeatherComponent from '@/components/WeatherComponent'
import NasaComponent from '@/components/NasaComponent'

export default {
    components:{
        WeatherComponent, NasaComponent
    },
    name: 'App',
    data(){
        return	{
            is_day: '',
            defaultBackgroud: true,
        }
    },
    methods: {
        checkBackgroud(){
           if (!this.defaultBackgroud){
               if (this.is_day) return 'day';
               else return 'night';
           }
        },
        getBackground(background, is_day){
            this.defaultBackgroud = background;
            this.is_day = is_day;
        },
    }
}
</script>

<style>
    * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

#app {
    background: none;
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
}

main {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.24), rgba(0, 0, 0, 0.75));
}

main.day {
    background: url('https://www.gorodlaminata.ru/upload/iblock/40f/40f824f9cd0fdca61b99e17bff121b6b.jpg');
}

main.night {
    background: url('https://catherineasquithgallery.com/uploads/posts/2021-02/1612554383_209-p-serii-gradientnii-fon-fotoshop-251.jpg  ');
    background-size: cover;
    background-position: bottom;
}

</style>
