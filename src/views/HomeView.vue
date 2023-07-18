<template>
  <div class="mt-12 pt-12">
    <v-form @submit.prevent>
      <v-card
        class="mx-auto pa-12 pb-8"
        elevation="8"
        width="80%"
        max-width="440px"
        rounded="lg"
      >
        <v-text-field
          color="success"
          density="compact"
          label="Số báo danh"
          prepend-inner-icon="mdi-email-outline"
          variant="outlined"
          v-model="id"
          :rules="rules"
        >
        </v-text-field>

        <v-btn
          @click="handle(id)"
          block
          class="mb-8"
          color="success"
          size="large"
          variant="tonal"
          type="submit"
        >
          TRA CỨU
        </v-btn>
      </v-card>
    </v-form>
  </div>
</template>

<script>
export default {
  data: () => ({
    id: "",
    rules: [
      (v) => !!v || "Bạn không điền làm sao tôi tra được??",
      (v) => v.length == 8 || "Điền đúng số báo danh bạn ơi!!",
    ],
  }),
  methods: {
    handle(id) {
      this.$router.push(`/${id}`);
    },
  },
};
</script>

<style></style>
