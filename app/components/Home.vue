<template>
    <Page>
        <ActionBar>
            <SearchBar
                hint="Где вы находитесь?"
                v-model="query"
                @submit="getCoords"
                width="300"
            />
        </ActionBar>

        <GridLayout>
            <TabView>
                <TabViewItem title="Сегодня">
                    <HtmlView :html="weather ? `
                        <div>
                            <h1>Сейчас</h1>
                            <h4>Температура сейчас: <i>${weather.fact.temp} С</i></h4>
                            <h4>Ощущается как: <i>${weather.fact.feels_like} С</i></h4>
                            <h4>Скорость ветра: <i>${weather.fact.wind_speed} м/с</i></h4>
                            <h4>Влажность: <i>${weather.fact.humidity} %</i></h4>
                            <br/>
                            <br/>
                            <h1>Днем</h1>
                            <h4>Температура: <i>${weather.forecasts[0].parts.day_short.temp} С</i></h4>
                            <h4>Ощущается как: <i>${weather.forecasts[0].parts.day_short.feels_like} С</i></h4>
                            <h4>Скорость ветра: <i>${weather.forecasts[0].parts.day_short.wind_speed} м/с</i></h4>
                            <h4>Влажность: <i>${weather.forecasts[0].parts.day_short.humidity} %</i></h4>
                            <br/>
                            <br/>
                            <h1>Ночью</h1>
                            <h4>Температура: <i>${weather.forecasts[0].parts.night_short.temp} С</i></h4>
                            <h4>Ощущается как: <i>${weather.forecasts[0].parts.night_short.feels_like} С</i></h4>
                            <h4>Скорость ветра: <i>${weather.forecasts[0].parts.night_short.wind_speed} м/с</i></h4>
                            <h4>Влажность: <i>${weather.forecasts[0].parts.night_short.humidity} %</i></h4>
                            <br/>
                        </div>
                    ` : `<div>Введите город</div>`"
                    />
                </TabViewItem>
                <TabViewItem title="Завтра">
                    <HtmlView :html="weather ? `
                        <div>
                            <h1>Днем</h1>
                            <h4>Температура: <i>${weather.forecasts[1].parts.day_short.temp} С</i></h4>
                            <h4>Ощущается как: <i>${weather.forecasts[1].parts.day_short.feels_like} С</i></h4>
                            <h4>Скорость ветра: <i>${weather.forecasts[1].parts.day_short.wind_speed} м/с</i></h4>
                            <h4>Влажность: <i>${weather.forecasts[1].parts.day_short.humidity} %</i></h4>
                            <br/>
                            <br/>
                            <h1>Ночью</h1>
                            <h4>Температура: <i>${weather.forecasts[1].parts.night_short.temp} С</i></h4>
                            <h4>Ощущается как: <i>${weather.forecasts[1].parts.night_short.feels_like} С</i></h4>
                            <h4>Скорость ветра: <i>${weather.forecasts[1].parts.night_short.wind_speed} м/с</i></h4>
                            <h4>Влажность: <i>${weather.forecasts[1].parts.night_short.humidity} %</i></h4>
                            <br/>
                        </div>
                    ` : `<div>Введите город</div>`"
                    />
                </TabViewItem>
                <TabViewItem title="Послезавтра">
                    <HtmlView :html="weather ? `
                        <div>
                            <h1>Днем</h1>
                            <h4>Температура: <i>${weather.forecasts[2].parts.day_short.temp} С</i></h4>
                            <h4>Ощущается как: <i>${weather.forecasts[2].parts.day_short.feels_like} С</i></h4>
                            <h4>Скорость ветра: <i>${weather.forecasts[2].parts.day_short.wind_speed} м/с</i></h4>
                            <h4>Влажность: <i>${weather.forecasts[2].parts.day_short.humidity} %</i></h4>
                            <br/>
                            <br/>
                            <h1>Ночью</h1>
                            <h4>Температура: <i>${weather.forecasts[2].parts.night_short.temp} С</i></h4>
                            <h4>Ощущается как: <i>${weather.forecasts[2].parts.night_short.feels_like} С</i></h4>
                            <h4>Скорость ветра: <i>${weather.forecasts[2].parts.night_short.wind_speed} м/с</i></h4>
                            <h4>Влажность: <i>${weather.forecasts[2].parts.night_short.humidity} %</i></h4>
                            <br/>
                        </div>
                    ` : `<div>Введите город</div>`"
                    />
                  </TabViewItem>
              </TabView>
        </GridLayout>
    </Page>
</template>

<script>
// Импортируем модуль Http для запросов по API
import { Http } from '@nativescript/core';

export default {
    data() {return {
        query: '',
        weather: null, 
        lon: 0,
        lat: 0,
    }},
    methods: {
        // Координаты города по названию
        getCoords() {
            Http.getJSON('https://nominatim.openstreetmap.org/search.php?format=jsonv2&city=' + this.query)
                .then(
                    (result) => {
                        if (result) {
                            this.lon = result[0].lon;
                            this.lat = result[0].lat;
                            this.search();
                        }
                    })
        },
        search() {
            // Погода по координатам
            Http.request({
                url: `https://api.weather.yandex.ru/v2/forecast?lat=${this.lat}&lon=${this.lon}`,
                method: 'GET',
                headers: { "X-Yandex-API-Key": '3c5f101e-04de-4c49-965f-54c8eb3f8ea2' },
            }).then(
                (response) => {
                    this.weather = JSON.parse(response.content);
                },
            )
        },
    },
};
</script>