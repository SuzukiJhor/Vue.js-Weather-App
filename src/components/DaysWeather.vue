<template>
    <div class="days-tab text-center">
        <div class="loading" v-if="loading">Loading...</div>
        <ul class="p-0">
            <li class="li_active" v-for="day in forecast" :key="day.date">
                <div class="py-3"><img :src="day.iconUrl"></div>
                <div class="py-3">{{ day.date }}</div>
                <div class="py-3">{{ day.temperature }}&deg;C</div>
            </li>

        </ul>
    </div>
</template>

<script>
export default {
    name: 'DaysWeather',
    props: {
        cityname: String
    },
    data() {
        return {
            forecast: [],
            loading: true,
            iconUrl: null,
        }
    },
    mounted() {
        this.fetchWeatherData();
    },
    methods: {
        async fetchWeatherData() {
            const apiKey = '804510e0033e4d65eefbf1ff697d676c';
            const city = this.cityname;
            const apiUrl = `https://api.openweathermap.org/data/2.5/forecast?q=${city}&units=metric&appid=${apiKey}`;
            let filteredData = ''

            try {
                const res = await fetch(apiUrl);
                if (res.ok) {
                    const resJson = await res.json();
                    const forecastData = resJson.list;

                    filteredData = forecastData.map(item => {
                        return {
                            date: item.dt_txt.split(" ")[0],
                            temperature: Math.round(item.main.temp),
                            description: item.weather[0].description,
                            iconUrl: `https://api.openweathermap.org/img/w/${item.weather[0].icon}.png`
                        }
                    }).slice(1, 4);
                } else {
                    console.error('Erro na resposta da API:', res.status, res.statusText);
                }
            } catch (error) {
                console.error('Erro na requisição:', error);
            }

            this.forecast = filteredData;
            this.loading = false;
        }
    }
}

</script>

<style scoped>
.days-tab {
    width: 90%;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2), 0 6px 20px rgba(0, 0, 0, 0.19);
    border-radius: 20px;
    width: 90%;
    margin: auto;
}

.loading {
    color: #fff;
}

ul {
    margin: 0;
}

li {
    display: inline-block;
    list-style: none;
    height: 100%;
    width: 21%;
    font-size: 1vw;
    line-height: 1.2;
}

span {
    display: block;
    margin-bottom: 5px;
}

.li_active {
    background: #253d5c;
    color: #222831;
    border-radius: 10px;
    margin: 0.5rem;
    color: #fff;
    font-weight: 600;
}

.li_active:hover {
    transform: scale(1.2);
    transition: transform 0.1s ease;
}

.li_active_temp {
    display: inline-block;
    background-color: #222831;
    color: #ffffff;
    transition: background-color0.5s;
    border-radius: 10px;
}
</style>