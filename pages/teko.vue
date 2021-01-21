<template>
  <div class="center examplex">
    <vs-navbar text-white :color="active" center-collapsed v-model="active">
      <template #left>
        <img
          class="logo"
          src="https://tekoventures.vn/wp-content/uploads/2018/09/logo-teko.png"
          alt=""
        />
      </template>
    </vs-navbar>
    <div class="square">
      <div>
        <div>
          Website đang trong quá trình hoàn thiện. Trong thời gian chờ đợi bạn
          có thể mua hàng tại:
        </div>
        <div class="flex">
          <vs-button href="https://phongvu.vn"> Phong Vũ </vs-button>
          <vs-button href="https://vnshop.vn" danger> Vnshop </vs-button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.flex {
  display: flex;
  justify-content: center;
  margin-top: 16px;
}
.examplex {
  background: #fff;
}
.logo {
  max-height: 30px;
}
.square {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 16px;
}
</style>

<script>
export default {
  data: () => ({
    active: 'primary',
  }),
}
</script>