<template>
  <v-app>
    <v-app-bar color="success">
      <h3 class="text-center ma-auto">TRA CỨU ĐIỂM THI TỐT NGHIỆP THPT 2023</h3>
    </v-app-bar>
    <v-main>
      <router-view />
    </v-main>

    <v-footer
      color="success"
      class="mt-12 text-center d-flex flex-column px-12"
    >
      <div class="mt-3 mb-2 mx-12 px-12 text-justify">
        <strong>8:00AM 17/07/2023</strong>

        <p>
          🥱 Cũng hóng điểm như mọi người và khá vừa lòng với cái điểm số của
          bản thân hiển thị trên web, cũng chụp màn hình đi flex các kiểu(mặc dù
          điểm thấp vc). Lúc sau, tôi khá là không ưng cái website tra cứu điểm
          của BDG, nhìn nó vừa xấu vừa loằng ngoằng. Cái bảng điểm cần to, cần
          ấn tượng thì lại cho bé tí ti bên dưới, flex không đã. Vì vậy tôi
          quyết định tạo ra cái trang web củ chuối này với một vài thứ hay ho
          hơn website của bộ
        </p>

        <p>
          🙄 Đây cũng là 1 website tra cứu điểm lấy dữ liệu trực tiếp từ API của
          Bộ Giáo dục. Nó là sản phẩm của cả 1 buổi chiều ngồi vô tri trước cái
          màn hình laptop của tôi, hy vọng sẽ giúp các bạn có những pha flex
          đỉnh cao.
        </p>

        <p>
          😛 Nếu flex thành công hoặc thấy thích website này, bạn có thể mời tôi
          1 ly cafe bằng cách nhấn vào đây
          <a href="https://me.momo.vn/donateKhanhPahm">👉☕</a>
        </p>
      </div>

      <div class="mt-3">
        <v-btn
          class="mx-5"
          icon="mdi-facebook"
          variant="text"
          href="https://facebook.com/giakhanh30125"
        ></v-btn>
        <v-btn
          class="mx-5"
          icon="mdi-github"
          variant="text"
          href="https://github.com/liam-parry"
        ></v-btn>
        <v-btn
          class="mx-5"
          icon="mdi-youtube"
          variant="text"
          href="https://youtube.com/@codelin911"
        ></v-btn>
      </div>

      <div class="text-overline">
        <span>18/07/2023</span> —
        <strong
          >by <a href="http://facebook.com/giakhanh30125">KhanhPahm</a></strong
        >
      </div>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    //
  }),
};
</script>

<style>
a {
  color: inherit;
  text-decoration: inherit; /* no underline */
  transition: all 0.3s;
}
a:hover {
  font-size: 1.5em;
}
</style>
