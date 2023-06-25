<template>
	<div class="flex flex-col flex-1 items-center">

		<!-- Banner -->
		<div
			v-if="route.query.preview"
			class="text-white p-4 bg-weather-secondary w-full text-center">
			<p>
				You are currently previewing this city, click the "+"
				icon to start tracking this city.
			</p>
		</div>

		<!-- Weather Overview -->
		<div class="flex flex-col items-center text-white py-12">
      <h1 class="text-4xl mb-2">{{ route.params.city }}</h1>
      <p class="text-sm mb-12">
        {{
          new Date(weatherData.currentTime).toLocaleDateString(
            "en-us",
            {
              weekday: "short",
              day: "2-digit",
              month: "long",
            }
          )
        }}
        {{
          new Date(weatherData.currentTime).toLocaleTimeString(
            "en-us",
            {
              timeStyle: "short",
            }
          )
        }}
      </p>
      <p class="text-8xl mb-8">
        {{ Math.round(weatherData.main.temp) }}&deg;
      </p>
      <p>
        Feels like
        {{ Math.round(weatherData.main.feels_like) }} &deg;
      </p>
      <p class="capitalize">
        {{ weatherData.weather[0].description }}
      </p>
      <img
        class="w-[150px] h-auto"
        :src="
          `http://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`
        "
        alt=""
      />
    </div>

    <hr class="border-white border-opacity-10 border w-full" />

    <div
      @click="removeCity"
      class="flex items-center gap-2 py-12 text-white cursor-pointer duration-150 hover:text-red-500"
      >
      <i class="fa fa-trash" aria-hidden="true"></i>
      <p>Remove sity</p>
    </div>

	</div>
</template>

<script setup>
import axios from "axios";
import { useRoute, useRouter } from "vue-router";

const route = useRoute();
const getWeatherData = async () => {
	try {
		const weatherData = await axios.get(
			`https://api.openweathermap.org/data/2.5/weather?lat=${route.query.lat}&lon=${route.query.lng}&appid=c0880aa2361e8276f908ca9e366b3433&units=metric`
		);

		// cal current date & time
		const localOffset = new Date().getTimezoneOffset() * 60000;
		const utc = weatherData.data.dt * 1000 + localOffset;
		weatherData.data.currentTime = utc + 1000 * weatherData.data.timezone;

    await new Promise((res) => setTimeout(res, 1000));

		return weatherData.data;
	} catch (err) {
		console.log(err);
	}
};
const weatherData = await getWeatherData();

const router = useRouter();
const removeCity = () => {
  const cities = JSON.parse(localStorage.getItem("savedCities"));
  const updatedCities = cities.filter(
    (city) => city.id !== route.query.id
  );
  localStorage.setItem(
    "savedCities",
    JSON.stringify(updatedCities)
  );
  router.push({
    name: "home",
  });
};
</script>