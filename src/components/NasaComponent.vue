<template>
    <div class="nasa">
            <h3 class="nasa-title__nasa">
                Данные, полученные по API NASA 
            </h3>
            <button 
                class="nasa-btn__nasa"
                @click="getNasa"
            >
                Получить данные по API NASA 
            </button>
            <transition name="elems-nasa">
                <div key="this.key" class="nasa-elems__nasa">
                    <div :class="visible" class="nasa-elems__nasa-elem__nasa">{{ this.nasa.title }} </div>
                    <p :class="visible" class="nasa-elems__nasa-elem__nasa"><img :src="this.nasa.url" alt=""></p>
                    <p :class="visible" class="nasa-elems__nasa-elem__nasa"> {{ this.nasa.date }} </p>
                </div>
            </transition>
        </div> 
</template>

<script>
import axios from 'axios';

    export default {
        name : 'Nasa',
        data(){
            return {
                api_key_nasa : 'gXj5IgX5hRZpDpIFtPreJpkAXhbFV7trJs6K2F0J',
                url_base_nasa : 'https://api.nasa.gov/planetary/apod?api_key=',
                visible: 'false',
                key: Date.now(),
                nasa: {},
            }
        },
        methods: {
            getNasa () {
                axios.get(`${this.url_base_nasa}${this.api_key_nasa}`)
                .then(res => {
                    return res.data;
                }).then(this.setNasaResults)
                .catch(() => {
                    alert('Не удалось получить к API NASA!')
                });
            },
            setNasaResults (results) {
                this.visible = true;
                this.nasa = results;
            },
        }
    }
</script>

<style scoped>

.nasa-btn__nasa {
    padding: 15px;
    cursor: pointer;
    margin: 20px 0;
    border-radius: 5px;
    border: 0;
    color: white;
    display: flex;
    background-color: rgb(233 141 141);
    align-items: center;
    transition: .3s;
}

.nasa-elems__nasa-elem__nasa.false {
    display: none;
}

.nasa {
    margin-top: 20px;
    width: 100%;
    border-radius: 15px;
    display: flex;
    margin: 0 auto;
    margin-bottom: 30px;
    flex-direction: column;
    align-items: center;
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

.nasa-elems__nasa-elem__nasa {
    display: block;
    border-radius: 10px;
    font-weight: 500;
    padding: 10px;
    margin: 10px auto;
}

.nasa-elems__nasa-elem__nasa img {
    border-radius: 5px;
}

@media (max-width: 407px) {
    .nasa {
        font-size: 17px;
    }
}

@media (max-width: 1000px) {
    .nasa-elems__nasa-elem__nasa img {
        height: auto;
        width: 100%;
    }
}

</style>