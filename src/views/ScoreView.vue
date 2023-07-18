<template>
  <div class="mt-12">
    <v-card
      class="mx-auto pa-2"
      elevation="8"
      width="80%"
      max-width="440px"
      rounded="lg"
    >
      <v-card-title class="text-center">
        <h4>Biểu đồ điểm của {{ $route.params.id }}</h4>
      </v-card-title>
      <apexchart
        type="radar"
        height="400"
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
      //console.log(window.location.href);
      navigator.clipboard.writeText(window.location.href).then(() => {
        alert(
          "Đã copy link đến trang hồ sơ của bạn, bạn có thể gửi đường link này cho bạn bè"
        );
      });
    },
    /* async submit(id, capt, token) {
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
    }, */
    async getData(id) {
      const categories = [
        "Toán",
        "Văn",
        "Anh",
        "Lý",
        "Hoá",
        "Sinh",
        "Sử",
        "Địa",
        "GDCD",
      ];
      const res = await fetch(
        `https://tienphong.vn/api/diemthi/get/result?type=0&keyword=${id}&kythi=THPT&nam=2023&cumthi=0`
      );

      const response = await res.json();
      //console.log(response);
      const tdValues = response.data.results.split(/<\s*\/?\s*td\s*[^>]*>/);

      console.log(response.data.results);
      if (response.data.results == "\n") {
        alert("Số báo danh không hợp lệ!!!!!!");
        window.location.href = "/";
      }
      let value = [];
      for (let i = 3; i < 20; i = i + 2) {
        value.push(tdValues[i]);
      }
      for (let i = 0; i < 9; i++) {
        if (value[i] != "") {
          this.series[0].data.push(value[i]);
          this.chartOptions.xaxis.categories.push(categories[i]);
        }
      }
      //console.log(value);
    },
  },
  mounted() {
    this.getData(this.$route.params.id);
  },
};
</script>

<style lang="scss" scoped></style>
