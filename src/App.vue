<script>
import axios from 'axios'

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null
    
    } 
  },
  computed: {
    cityName () {
      return "«" + this.city + "»"
    },
    showTemp() {
      return "Температура:" + this.info.main.temp
    },
    showFeelaLike() {
      return "Ощущается как:" + this.info.main.feels_like
    },
    showMinTemp() {
      return "Минимальная температура:" + this.info.main.temp_min
    },
    showMaxTemo() {
      return "Максимальная температура:" + this.info.main.temp_max
    }
  },
  methods: {
    getWeather () {
        if(this.city.trim().length < 2) {
          this.error = "Нужно более одного символа"
          return false
        }
        this.error = ""

        axios.get(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=ec889b21111b08e5e682effe0e583cf1`)
        .then(res =>(this.info = res.data))
    }
  },
} 
</script>  

<template>
<div class="wrapper"> 
  <h1>Прогноз погоды</h1>
  <p>Узнать погоду в {{ city  == "" ? "вашем городе" : cityName}}</p>
  <input type="text" v-model="city" placeholder="Введите город"> 
  <button v-if="city != ''" @click="getWeather()">Получить данные о погоде</button> 
  <button disabled v-else>Введите названия города</button> 
  <p class="error">{{ error }}</p>

  <div class="pogoda" v-if="info !=null">
     <p>{{ showTemp}}</p>
     <p>{{ showFeelaLike}}</p>
     <p>{{ showMinTemp}}</p>
     <p>{{ showMaxTemo}}</p>
  </div>
</div> 
</template>

<style scoped>
.error {
  color: rgb(99, 0, 0);
}

.wrapper {
  width: 900px;
  height: 500px;
  border-radius: 50px;
  padding: 20px;
  background-color: rgb(115, 206, 241);
  text-align: center;
  color: rgb(250, 116, 183);
}
.wrapper h1 {
margin-top: 50px;
}
.wrapper p {
  margin-top: 20px;
}
.wrapper input {
  margin-top: 30px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid rgb(250, 116, 183);
  color: rgb(250, 116, 183);
  font-size: 14px;
  padding: 5px 8px;
  outline: none;
}

.wrapper input:focus {
  border-bottom-color: aqua;
}
.wrapper button {
  background: aqua;
  color: rgb(5, 5, 5);
  border-radius: 10px;
  border: 1px solid rgb(250, 142, 142);
  padding: 7px 10px;
  margin-left: 10px;
  cursor: pointer;
  transition: margin-right 2s ease-in-out ;
}
.wrapper button:hover {
background-color: rgb(250, 116, 183);
transform: scale(1.1);
}
.wrapper button:disabled {
  background: red;
  cursor: not-allowed;
}
.pogoda {
  color: rgb(250, 116, 183);
  border: 2px solid;
  border-bottom: solid;
  border-radius: 100px;

}

</style>