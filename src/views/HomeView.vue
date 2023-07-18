<template>
  <div class="mt-12 pt-12">
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
        :rules="[rules.required]"
      >
      </v-text-field>

      <v-btn
        @click="handle(id)"
        block
        class="mb-8"
        color="success"
        size="large"
        variant="tonal"
      >
        TRA CỨU
      </v-btn>
    </v-card>
  </div>
</template>

<script>
export default {
  data: () => ({
    id: "",
    rules: {
      required: (value) =>
        !!value || "Bạn chưa nhập sô báo danh, làm sao tôi tra??",
    },
  }),
  methods: {
    handle(id) {
      this.$router.push(`/${id}`);
    },
  },
};
</script>

<style></style>
