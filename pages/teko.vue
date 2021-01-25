<template>
  <div class="center examplex">
    <vs-navbar class="navbar" text-white :color="active" center-collapsed v-model="active">
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
        <h3>Sản phẩm của chúng tôi</h3>
        <div class="out-products">
          <vs-card type="2">
            <template #img>
              <img src="~/assets/vnshop.jpg" alt="">
            </template>
          </vs-card>
          <vs-card type="2">
            <template #img>
              <img src="~/assets/phongvu.jpg" alt="">
            </template>
          </vs-card>
          <vs-card type="2">
            <template #img>
              <img src="~/assets/vinmart.png" alt="">
            </template>
          </vs-card>
          <vs-card type="2">
            <template #img>
               <img src="~/assets/meat-deli.png" alt="">
            </template>
          </vs-card>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.out-products {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 8px;
}
.flex {
  display: flex;
  justify-content: center;
  margin-top: 16px;
}
.examplex {
  background: #fff;
  max-width: 480px;
  margin: auto;
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