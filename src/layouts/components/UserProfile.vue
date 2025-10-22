<script setup>
import AuthProvider from '@/views/pages/authentication/AuthProvider.vue'
import avatar1 from '@images/avatars/colorful.png'
import { onMounted, ref, watch } from 'vue'

const email = ref('Santana')
const password = ref('admin123123123123')
const isPasswordVisible = ref(false)
const rememberMe = ref(false)
const refVForm = ref()


const isLoggedIn = ref(false)
const isDialogVisible = ref(false)
const isMenuVisible = ref(false)

const login = () => {
  if ( password.value === 'admin123123123123') {
    isLoggedIn.value = true
    isDialogVisible.value = false
    isMenuVisible.value = true 
    localStorage.setItem('isLoggedIn', 'true') 
    localStorage.setItem('userEmail', email.value)
    location.reload()
  } else {
    alert('Нэвтрэх нэр эсвэл нууц үг буруу байна.')
  }
}
onMounted(() => {
  const savedLoginState = localStorage.getItem('isLoggedIn')
  if (savedLoginState === 'true') {
    isLoggedIn.value = true
  }
})
const handleAvatarClick = () => {
  console.log('Avatar clicked. isLoggedIn:', isLoggedIn.value)
  if (isLoggedIn.value) {
    console.log('Opening menu...')
    isMenuVisible.value = true
    isDialogVisible.value = false
  } else {
    console.log('Opening dialog...')
    isDialogVisible.value = true
    isMenuVisible.value = false
  }
}
const logout = () => {
  isLoggedIn.value = false
  isMenuVisible.value = false
  localStorage.removeItem('isLoggedIn') 
  localStorage.removeItem('userEmail')
  location.reload()
}
watch(isLoggedIn, val => {
  if (!val) {
    isMenuVisible.value = false
  } else {
    isDialogVisible.value = false
  }
})
</script>

<template>

<VMenu
  v-model="isMenuVisible"
  location="bottom end"
  width="230"
  offset="10"
  :close-on-content-click="false"
>
  <template #activator="{ props }">
    <VAvatar
      v-bind="props"
      class="cursor-pointer"
      @click="handleAvatarClick"
    >
      <VImg :src="avatar1" />
    </VAvatar>
  </template>
  <VList>
    <VListItem>
      
      <template #prepend>
        <VListItemAction>
          <VBadge dot location="bottom right" offset-x="3" offset-y="3" color="success">
            <VAvatar>
              <VImg :src="avatar1" />
            </VAvatar>
          </VBadge>
        </VListItemAction>
      </template>
      <VListItemTitle class="font-weight-semibold ms-1">{{ email }}</VListItemTitle>
      <VListItemSubtitle class="ms-1">Админ</VListItemSubtitle>
    </VListItem>

    <VDivider class="my-2" />

    <VListItem link>
      <template #prepend>
        <VIcon class="me-2" icon="tabler-user" size="22" />
      </template>
      <VListItemTitle>Профайл</VListItemTitle>
    </VListItem>

    <VListItem link>
      <template #prepend>
        <VIcon class="me-2" icon="tabler-settings" size="22" />
      </template>
      <VListItemTitle>Тохиргоо</VListItemTitle>
    </VListItem>

    <VDivider class="my-2" />

    <VListItem link @click="logout">
      <template #prepend>
        <VIcon class="me-2" icon="tabler-logout" size="22" />
      </template>
      <VListItemTitle>Гарах</VListItemTitle>
    </VListItem>
  </VList>
</VMenu>


  <VDialog v-model="isDialogVisible" max-width="500" persistent>
    <DialogCloseBtn @click="isDialogVisible = false" />
    <VCard title="Нэвтрэх">
      <VCardText>
        <VForm ref="refVForm" @submit.prevent="login">
          <VRow>
            <VCol cols="12">
              <AppTextField v-model="email" label="Email" type="email" autofocus />
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
                <VCheckbox v-model="rememberMe" label="Намайг сануулах" />
                <a class="text-primary ms-2 mb-1" href="#">Нууц үг мартсан?</a>
              </div>

              <VBtn block type="submit">Нэвтрэх</VBtn>
            </VCol>

            <VCol cols="12" class="text-center">
              <span>Шинэ хэрэглэгч үү?</span>
              <a class="text-primary ms-2" href="#">Аккоунд үүсгэх</a>
            </VCol>

            <VCol cols="12" class="d-flex align-center">
              <VDivider />
              <span class="mx-4">эсвэл</span>
              <VDivider />
            </VCol>

            <VCol cols="12" class="text-center">
              <AuthProvider />
            </VCol>
          </VRow>
        </VForm>
      </VCardText>
    </VCard>
  </VDialog>
</template>
