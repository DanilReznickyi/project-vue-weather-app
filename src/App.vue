<script>
import axios from 'axios'
export default{
    data(){
        return{
            city: '',
            error: '',
            info: null
        }
    },
    computed: {
        cityName(){
            return "«" + this.city + "»"
        },
        showTemp(){
            return "Temperature: " + this.info.main.temp 
        },
        showFeelsLike(){
            return "Feels like: " + this.info.main.feels_like 
        },
        showMinTemp(){
            return "Minimum temperature: " + this.info.main.temp_min
        },
        showMaxTemp(){
            return "Maximum temperature: " + this.info.main.temp_max
        },
    },
    methods: {
        getWeather() {
            if(this.city.trim().length < 2){
                this.error = "Misnomer"
                return false
            }
            this.error = ""
            axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=2db4a3b0193512569273dd45ed0c41c9`)
            .then(res => (this.info = res.data))
        }
    }
}
</script>

<template>
    <div class="wrapper">
        <h1>Weather App</h1>
        <p>Find out the weather {{ city == "" ? "in your city" : "in " + cityName }}</p>
        <input type="text" v-model="city" placeholder="Type a city name">
        <button v-if="city != ''" @click="getWeather()">Get weather</button>
        <button disabled v-else>Type a city name</button>
        <p class="error">{{ error }}</p>
        <div v-if="info != null">
            <p>{{ showTemp }}</p>
            <p>{{ showFeelsLike }}</p>
            <p>{{ showMinTemp }}</p>
            <p>{{ showMaxTemp }}</p>
        </div>
    </div>
</template>
<style scoped>
.error{
    color: #d03939;
}
    .wrapper{
        width: 900px;
        height: 500px;
        padding: 20px;
        border-radius: 50px;
        background: #1f0f24;
        text-align: center;
        color: #fff;
    }
    .wrapper h1{
        margin-top: 50px;
    }
    .wrapper p{
        margin-top: 20px;
    }
    .wrapper input{
        margin-top: 30px;
        background-color: transparent;
        border: 0;
        border-bottom: 2px solid #110813;
        color: #fcfcfc;
        font-size: 14px;
        padding: 5px 8px;
        outline: none;
    }
    .wrapper input:focus{
        border-bottom-color:#6e2d7d ;
    }
    .wrapper button{
        background: #e3bc4b;
        color: #fff;
        border-radius: 10px;
        font-size: 16px;
        border: 2px solid #b99935;
        padding: 10px 35px;
        margin-left: 20px;
        cursor: pointer;
        transition: transform 500ms ease;
    }
    .wrapper button:disabled{
        background: #746027;
        cursor: not-allowed;
    }
    .wrapper button:hover{
        transform: scale(1.1) translateY(-5px);
    }
</style>
