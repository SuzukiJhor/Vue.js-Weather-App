<template>
    <div class="container p-0">
        <div class="d-flex">
            <div class="card main-div w-100">
                <div class="p-3">
                    <h2 class="mb-1 day">Today</h2>
                    <p class="text-light date mb-0">{{ date }}</p>
                    <small>{{ time }}</small>
                    <h2 class="place"><i class="fa fa-location">{{ name }} <small>{{ country }}</small></i></h2>
                    <div class="temp">
                        <h1 class="weather-temp">{{ temperature }}º</h1>
                        <h2 class="text-light">{{ description }} <img :src="iconUrl"></h2>
                    </div>
                </div>
            </div>
            <div class="card card-2 w-100">
                <table class="m-4">
                    <tbody>
                        <tr>
                            <th>Sea Level</th>
                            <td v-if="sea_level > 0">{{ sea_level }}</td>
                            <td v-else>Null</td>
                        </tr>
                        <tr>
                            <th>Humidity</th>
                            <td v-if="humidity > 0">{{ humidity }}</td>
                            <td v-else>Null</td>
                        </tr>
                        <tr>
                            <th>Wind</th>
                            <td v-if="wind > 0">{{ wind }}</td>
                            <td v-else>Null</td>
                        </tr>
                    </tbody>
                </table>

                <DaysWeather :cityname="cityname" />

                <div id="div_form" class="d-flex m-3 justify-content-center">
                    <form action="">
                        <input @click="changeLocation" type="button" value='Change Location'
                            class="btn change-btn btn-primary">
                    </form>
                </div>
            </div>
        </div>

    </div>
</template>

<script>

import DaysWeather from './DaysWeather.vue';

export default {
    name: 'MyWeather',
    components: {
        DaysWeather
    },
    props: {
        city: String
    },
    data() {
        return {
            cityname: this.city,
            temperature: null,
            description: null,
            iconUrl: null,
            date: null,
            time: null,
            name: null,
            monthNames: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
            country: null,
            sea_level: null,
            wind: null,
            humidity: null

        }
    },
    methods: {

        changeLocation() {
            window.location.reload()
        },

        async getImageByCity(city) {
           
            const accessKey = 'rXgmo3usqDvKkCu4-Ksnhs_00zQssA-SF8ORkzm0vDg';
            const apiUrl = `https://api.unsplash.com/search/photos?page=1&query=${city}&client_id=${accessKey}`;
            let imgUrl;

            try {
                const response = await fetch(apiUrl, {
                    headers: {
                        'Authorization': `Client-ID ${accessKey}`,
                    },
                });

                if (response.ok) {
                  const  data = await response.json();

                  imgUrl = data.results[0].urls.regular

                } else {
                    console.error('Erro na resposta da API');
                }
            } catch (error) {
                console.error('Erro:', error);
            }

           const elementImg = document.querySelector('.main-div');
           elementImg.style.backgroundImage = `url(${imgUrl})`;
        }
    },

    async created() {
        const req = await fetch(
            `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=804510e0033e4d65eefbf1ff697d676c`
        );
        const res = await req.json();

        this.temperature = Math.round(res.main.temp);
        this.description = res.weather[0].description;
        this.name = res.name;
        this.wind = res.wind.speed;
        this.sea_level = res.main.sea_level;
        this.country = res.sys.country;
        this.humidity = res.main.humidity;
        this.iconUrl = `https://api.openweathermap.org/img/w/${res.weather[0].icon}`;
        const d = new Date();
        this.date = d.getDate() + '-' + this.monthNames[d.getMonth()] + '-' + d.getFullYear();
        this.time = d.getHours() + ':' + d.getMinutes() + ':' + d.getSeconds();
        this.getImageByCity(this.cityname);
    }

}
</script>

<style scoped>
.weather-temp {
    margin: 0;
    font-weight: 700;
    font-size: 4em;
}

h2.mb-1.day {
    font-size: 3rem;
    font-weight: 400;

}

.main-div {
    border-radius: 20px;
    color: #fff;
    background-image: url(https://images.unsplash.com/photo-1693497409544-02d6fd746712?auto=format&fit=crop&q=80&w=1887&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D);
    background-size: cover;
    background-position: center;
    background-blend-mode: overlay;
    background-color: rgba(0, 0, 0, 0.5);
    background-repeat: no-repeat;
}


.temp {
    position: absolute;
    bottom: 0;
}

.main-div:hover {
    transform: scale(1.1);
    transition: transform 0.5s ease;
    z-index: 1;
}

.card-2 {
    background-color: #212730;
    border-radius: 20px;
}

.card-details {
    margin-left: 19px;
}

.h1_left {
    position: absolute;
    bottom: 25px;
    left: 16px;
    font-size: 3vw;
    line-height: 1.2;
}

.h3_left {
    position: absolute;
    left: 16px;
    font-size: 2vw;
    line-height: 0.5;
}

.h3_left small {
    font-size: 1rem;
}

table {
    position: relative;
    left: 15px;
    border-collapse: separate;
    border-spacing: 15px;
    width: 85%;
    text-align: left;
    max-width: 600px;
    margin: 0 auto;
}

th,
td {
    font-size: 18px;
    color: #fff;
}

td {
    text-align: right;
}

table,
tr:hover {
    color: red;
}
</style>