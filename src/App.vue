<template>
  <div id="app" :class ="typeof weather.main != 'undefined' && pushMood() " >
    <div class="container">
      <main>
      <div class="search-bar">
        <input 
          type="text" 
          placeholder="Search location" 
          id="searchInput" 
          maxlength="20" 
          v-model="query"
          @keypress="getWeather"
        >
      </div>
      <div class="info" v-if="typeof weather.main != 'undefined' ">
        <h1 class="info__location">{{ weather.name }}, {{ weather.sys.country }}</h1>
        <p class="info__time"> {{ dataBuilder() }}</p>
        <div class="info__weather">
          <h1 class="info__temp">{{ Math.round(weather.main.temp) }}°c</h1>
        </div>
        <h2 class="info__mood">{{ weather.weather[0].description }}</h2>
      </div>
    </main>
    </div>
  </div>
</template>

<script>
export default {
  name : 'app',
  data(){
    return{
      api_key : '984c03285fb645acf5c4ca596fa60260',
      api_base : 'https://api.openweathermap.org/data/2.5/',
      query : '',
      weather : {},
    }
  },
  methods : {
    getWeather(a){
      if(a.key == 'Enter'){
        fetch(`${this.api_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
        .then ( res =>{
          return res.json()
        }).then(this.setResults)
      }
    },
    setResults(results){
      this.weather = results
      console.log(this.weather)
    },
    dataBuilder() {
      let d = new Date()
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "November", "December", "Febral", ]
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", ]

      let day = days[d.getDay()]
      let date = d.getDate()
      let month = months[d.getMonth()]
      let year = d.getFullYear()

      return `${day} ${date} ${month} ${year} `
    },
    pushMood() {
      var a = this.weather.weather[0].description
      var b = a.split(" ")
      if (b.includes("overcast")){
          return 'overcast'
      } else if (b.includes("broken")){
        return 'broken'
      } else if (b.includes("shower")){
        return 'shower'
      } else if (b.includes("clear")){
        return 'clear'
      } else if (b.includes("light")){
        return 'rain'
      } else if (b.includes("few")){
        return 'few'
      } else if (b.includes("scattered")){
        return 'scatted'
      } else if (b.includes("rain")){
        return 'rain'
      } else {
        return 'sun'
      }
    }
  }
}


</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@100;200;300;400;500;600;700;800;900&display=swap');

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  list-style-type: none;
  text-decoration-line: none;
}
#app{
    background-size: cover;
    background-image: url(./assets/main.jpeg);
    background-repeat: no-repeat;
    background-position: center;
}
#app.overcast{
  background-image: url(./assets/overCast_clouds.jpg);
}
#app.broken{
  background-image: url(./assets/broken_clouds.jpg);
}
#app.shower{
  background-image: url(./assets/shower_rain.jpg);
}
#app.storm{
  background-image: url(./assets/storm.jpg);
}
#app.clear{
  background-image: url(./assets/cloud.jpg);
}
#app.light{
  background-image: url(./assets/light_rain.jpg);
}
#app.few{
  background-image: url(./assets/few_clouds.jpg);
}
#app.scatted{
  background-image: url(./assets/scatted_image.jpg);
}
#app.rain{
  background-image: url(./assets/rain.gif);
}


main{
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-around;
  text-align: center;
  .search-bar{
    flex-basis: 50%;
    #searchInput{
      width: 80%;
      height: 64px;
      padding: 0 16px;
      border-radius: 16px 0 16px 0;
      border: 2px solid transparent;
      outline: none;
      font-size: 24px;
      color: #4d4d4d;
      font-family: 'Montserrat', sans-serif;
      transition: 0.4s ease-out;
      &:focus{
        border-radius: 0 16px 0 16px;
        border-color:#00bcd4;
        box-shadow: 0 0 29px rgba(37, 163, 217, 0.7)  ; 
      }
      &::placeholder{
        font-size: 22px;
      }
    }
  }
  .info{
    color: white;
    text-shadow: 3px 3px 6px rgba(48, 41, 48, 0.5) ; 
    flex-basis: 30%;
    background: #7fffd433;
    padding: 20px;
    border-radius: 36px;
    &__weather{
      display: inline-block;
      padding: 24px;
      margin: 16px 0;
      background: #f0fffd47;
      border-radius: 24px;
    }
    &__temp{
      font-size: 96px;
    }
    &__time{
      color: #e1e1e1;
      padding-top: 6px;
    }
    &__mood{
      font-size: 38px;
    }
  }
}
.container{
  max-width: 90%;
  margin: auto;
  height: 100vh;
  padding: 100px 0;
  font-family: 'Montserrat', sans-serif;
  display: flex;
  align-items: center;
  justify-content: center;
}

@media (max-width: 500px){
  main{
    flex-direction: column-reverse;
    gap: 42px;
  }
}
</style>
