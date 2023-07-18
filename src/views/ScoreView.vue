<template>
  <div class="mt-12">
    <v-form v-if="view" v-model="form" @submit.prevent="onSubmit">
      <v-card
        class="mx-auto pa-12 pb-8"
        elevation="8"
        width="80%"
        max-width="440px"
        rounded="lg"
      >
        <v-text-field
          color="success"
          append-inner-icon="mdi-reload"
          density="compact"
          label="Mã xác nhận"
          prepend-inner-icon="mdi-lock-outline"
          variant="outlined"
          @click:append-inner="getCaptcha()"
          v-model="captcha"
          :rules="[required]"
        >
        </v-text-field>

        <img v-if="img" :src="img" alt="alt" />

        <v-btn
          block
          class="mb-8"
          color="success"
          size="large"
          variant="tonal"
          :disabled="!form"
          type="submit"
          @click="submit($route.params.id, captcha, token)"
        >
          XÁC NHẬN
        </v-btn>
      </v-card>
    </v-form>
    <v-card
      class="mx-auto pa-3"
      elevation="8"
      width="80%"
      max-width="440px"
      rounded="lg"
      v-if="!view"
    >
      <v-card-title class="text-center">
        <h3>Biểu đồ điểm của {{ $route.params.id }}</h3>
      </v-card-title>
      <apexchart
        type="radar"
        height="440"
        :options="chartOptions"
        :series="series"
      ></apexchart>
      <v-card-actions class="float-right">
        <v-btn
          icon="mdi-share"
          color="success"
          variant="outlined"
          @click="handleShare()"
        >
        </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
import VueApexCharts from "vue3-apexcharts";
export default {
  components: {
    apexchart: VueApexCharts,
  },
  data: () => ({
    series: [
      {
        name: "Điểm",
        data: [],
      },
    ],
    chartOptions: {
      chart: {
        height: 350,
        type: "radar",
      },
      theme: {
        monochrome: {
          enabled: true,
          color: "#4caf50",
          shadeTo: "light",
          shadeIntensity: 0.65,
        }, // upto palette10
      },
      xaxis: {
        categories: [],
      },
      yaxis: {
        max: 10,
        min: 0,
      },
      dataLabels: {
        enabled: true,
        background: {
          enabled: true,
          borderRadius: 2,
        },
      },
    },
    view: true,
    img: "",
    captcha: "",
    form: false,
    token: "",
  }),
  methods: {
    handleShare() {
      console.log(window.location.href);
      navigator.clipboard.writeText(window.location.href).then(() => {
        alert(
          "Đã copy link đến trang hồ sơ của bạn, bạn có thể gửi đường link này cho bạn bè"
        );
      });
    },
    async submit(id, capt, token) {
      const url =
        "http://tracuudiem.thitotnghiepthpt.edu.vn:9999/service/api/v1/lookup-scores";
      const params = {
        identifyNumber: id,
        strCaptcha: capt,
        sessionId: token,
      };
      const res = await fetch(url, {
        method: "POST",
        headers: {
          "Content-type": "application/json",
          "Accept-Language": "vi-VN",
        },
        body: JSON.stringify(params),
      });
      const response = await res.json();
      console.log(response.data);

      const str = response.data.score;
      console.log(this.extractNumbersFromString(str));
      this.series[0].data = this.extractNumbersFromString(str);
      this.chartOptions.xaxis.categories = str
        .replace(/[\d.]+/g, "")
        .trim()
        .split(":")
        .map((item) => item.trim());

      this.view = false;

      //----------------------
      let cleaned = str
        .replace(/[\d.]+/g, "")
        .trim()
        .split(":")
        .map((item) => item.trim());

      console.log(cleaned);
    },
    extractNumbersFromString(str) {
      const regex = /\d+\.\d+/g;
      let numbers = str.match(regex).map(Number);
      return numbers;
    },
    onSubmit() {
      if (!this.form) return;
    },
    required(v) {
      return !!v || "Nhập hộ tôi cái mã xác nhận với!!!";
    },
    async getCaptcha() {
      const res = await fetch(
        "https://tracuudiem.thitotnghiepthpt.edu.vn:9999/service/api/v1/captcha"
      );
      const response = await res.json();
      console.log(response);
      if (response.code === 200) {
        this.img = "data:image/png;base64, " + response.data.imageCaptcha;
        this.token = response.data.sessionId;
      } else {
        this.$router.push("/");
      }
    },
  },
  mounted() {
    this.getCaptcha();
  },
};
</script>

<style lang="scss" scoped></style>
