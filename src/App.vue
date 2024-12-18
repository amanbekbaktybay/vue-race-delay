<template>
	<v-app>
		<v-container>
			<v-card class="pa-5">
				<v-card-title>Введите параметры для API</v-card-title>
				<v-card-text>
					<v-form v-model="valid" ref="form">
						<v-row v-for="(value, key) in params" :key="key" class="mb-3">
							<v-col cols="12" md="6">
								<v-text-field
									v-model="params[key]"
									:label="key"
									:rules="[rules.required]"
									:key="key"
								></v-text-field>
							</v-col>
						</v-row>
						<v-btn :disabled="!valid" @click="sendRequest" class="mt-4" color="primary">Отправить</v-btn>
					</v-form>
				</v-card-text>
				<v-card-actions>
					<v-alert v-if="response" type="success" border="top">
						Ответ от API: {{ response }}
					</v-alert>
				</v-card-actions>
			</v-card>
		</v-container>
	</v-app>
</template>

<script>
import axios from 'axios';

export default {
	data() {
		return {
			valid: false,
			params: {
				MONTH: "12",
				DAY_OF_MONTH: "1",
				DAY_OF_WEEK: "5",
				OP_UNIQUE_CARRIER: "B6",
				TAIL_NUM: "N828JB",
				DEST: "CHS",
				CRS_ELAPSED_TIME: "124",
				DISTANCE: "100000",
				CRS_DEP_M: "324",
				DEP_TIME_M: "345",
				CRS_ARR_M: "510",
				Temperature: "34",
				Dew_Point: "20",
				Humidity: "58",
				Wind: "W",
				Wind_Speed: "25",
				Wind_Gust: "30",
				Pressure: "29.86",
				Condition: "Mostly Cloudy",
				sch_dep: "9",
				sch_arr: "17",
				TAXI_OUT: "50",
			},
			response: null,
			rules: {
				required: (value) => !!value || "Обязательное поле",
			},
		};
	},
	methods: {
		async sendRequest() {
			try {
				const res = await axios.post("https://race-predict-production.up.railway.app/predict/", this.params, {
					headers: { "Content-Type": "application/json" },
				});
				this.response = res.data.predicted_delay;
			} catch (error) {
				console.error("Ошибка запроса:", error);
				this.response = "Ошибка запроса";
			}
		},
	},
};
</script>