<template>
  <div class="fullscreen-image">
    <div class="image-wrapper">
      <VImg :src="avatar1" cover class="background-img" />
      <div class="dark-overlay"></div>

        <h1 class="title-text" v-if="!isLoggedIn">
          WELCOME TO<br />
          PUBG<br />
          COMMUNITY
        </h1>

        <h1 class="title-text-else" v-else>
          WELCOME TO<br />
          PUBG<br />
          COMMUNITY<br />
          {{ userEmail }}
        </h1>
    </div>
    <VBtn
      v-if="!isLoggedIn"
      @click="isDialogVisible = true"
      class="button"
      size="x-large"
    >
      Нэвтрэх
    </VBtn>
    <div class="stats-container">
        <VCard class="stat-card" elevation="10">
          <VCardText>
            <h2>👥 359</h2>
            <p>Тоглогчийн тоо</p>
          </VCardText>
        </VCard>

        <VCard class="stat-card" elevation="10">
          <VCardText>
            <h2>🔥 10</h2>
            <p>Custom тэмцээн</p>
          </VCardText>
        </VCard>

        <VCard class="stat-card" elevation="10">
          <VCardText>
            <h2>🎮 78</h2>
            <p>Булаа тэмцээн</p>
          </VCardText>
        </VCard>
      </div>
  </div>
  <VDialog
    v-model="isDialogVisible"
    max-width="500"
  >
    <DialogCloseBtn @click="isDialogVisible = !isDialogVisible" />

    <VCard title="Нэвтрэх">
      <VCardText>
        <VRow>
        <VCardText>
          <VForm
            ref="refVForm"
            @submit="login"
          >
            <VRow>
              <VCol cols="12">
                <AppTextField
                  v-model="email"
                  label="Email"
                  type="email"
                  autofocus
                />
              </VCol>

              <VCol cols="12">
                <AppTextField
                  v-model="password"
                  label="Password"
                  :type="isPasswordVisible ? 'text' : 'password'"
                  :append-inner-icon="isPasswordVisible ? 'tabler-eye-off' : 'tabler-eye'"
                  @click:append-inner="isPasswordVisible = !isPasswordVisible"
                />

                <div class="d-flex align-center flex-wrap justify-space-between mt-2 mb-4">
                  <VCheckbox
                    v-model="rememberMe"
                    label="Намайг сануулах"
                  />
                  <a
                    class="text-primary ms-2 mb-1"
                    href="#"
                  >
                    Нууц үг мартсан?
                  </a>
                </div>

                <VBtn
                  block
                  type="submit"
                >
                  Нэвтрэх
                </VBtn>
              </VCol>

              <!-- create account -->
              <VCol
                cols="12"
                class="text-center"
              >
                <span>Шинэ хэрэглэгч уу?</span>

                <a
                  class="text-primary ms-2"
                  href="#"
                >
                  Аккоунд үүсгэх
                </a>
              </VCol>

              <VCol
                cols="12"
                class="d-flex align-center"
              >
                <VDivider />

                <span class="mx-4">эсвэл</span>

                <VDivider />
              </VCol>

              <!-- auth providers -->
              <VCol
                cols="12"
                class="text-center"
              >
                <AuthProvider />
              </VCol>
            </VRow>
          </VForm>
        </VCardText>
        </VRow>
      </VCardText>
    </VCard>
  </VDialog>
</template>

<script setup>
import avatar1 from '@/assets/images/background/pubg2.jpg';
import AuthProvider from '@/views/pages/authentication/AuthProvider.vue';
import { onMounted, ref } from 'vue';
import { VForm } from 'vuetify/components/VForm';

const isDialogVisible = ref(false)
const email = ref('SANTANA')
const password = ref('admin123123123123')

const isLoggedIn = ref(false)
const userEmail = ref('')
const isPasswordVisible = ref(false)
const refVForm = ref()
const rememberMe = ref(false)
const login = () => {
  if ( password.value === 'admin123123123123') {
    isLoggedIn.value = true
    isDialogVisible.value = false
    localStorage.setItem('isLoggedIn', 'true') 
    localStorage.setItem('userEmail', email.value)

  } else {
    alert('Нэвтрэх нэр эсвэл нууц үг буруу байна.')
  }
}
onMounted(() => {
  const storedEmail = localStorage.getItem('userEmail')
  const storedLoggedIn = localStorage.getItem('isLoggedIn')

  if (storedLoggedIn === 'true') {
    isLoggedIn.value = true
    userEmail.value = storedEmail || ''
  }
})
</script>

<style scoped>
.fullscreen-image {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

.image-wrapper {
  position: relative;
  width: 100%;
  height: 100%;
}

.background-img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  filter: blur(3px); 
  z-index: 0;
}

.dark-overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.65);
  z-index: 1;
}

.title-text { position: absolute; top: 30%; left: 30%; transform: translate(-50%, -50%); z-index: 2; color: white; font-size: 5rem; font-family: Arial, Helvetica, sans-serif; margin: 0; line-height: 1.1; user-select: none; }

.title-text-else { position: absolute; top: 33%; left: 30%; transform: translate(-50%, -50%); z-index: 2; color: white; font-size: 5rem; font-family: Arial, Helvetica, sans-serif; margin: 0; line-height: 1.1; user-select: none; }

.button {
  position: absolute;
  top: 45%;
  left: 16%;
  z-index: 3; 
}
.stats-container {
  position: absolute;
  bottom: 31.5%;
  left: 32%;
  transform: translateX(-50%);
  display: flex;
  gap: 1rem;
  z-index: 3;
  flex-wrap: wrap;
  justify-content: center;
}

.stat-card {
  background-color: rgba(255, 255, 255, 0.08);
  color: white;
  backdrop-filter: blur(8px);
  border-radius: 10px;
  min-width: 150px;
  text-align: center;
  padding: 1rem;
}
.stat-card h2 {
  margin: 0;
  font-size: 1.8rem;
  font-weight: bold;
  color: #cecece;
}

.stat-card p {
  margin: 0;
  font-size: 1rem;
  opacity: 0.85;
}

</style>
