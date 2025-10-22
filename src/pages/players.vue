<template>
        <div class="d-flex mb-6">
      <VRow>
        <template
          v-for="(data, id) in widgetData"
          :key="id"
        >
          <VCol
            cols="12"
            md="3"
            sm="6"
          >
            <VCard>
              <VCardText>
                <div class="d-flex justify-space-between">
                  <div class="d-flex flex-column gap-y-1">
                    <div class="text-body-1 text-high-emphasis">
                      {{ data.title }}
                    </div>
                    <div class="d-flex gap-x-2 align-center">
                      <h4 class="text-h4">
                        {{ data.value }}
                      </h4>
                      <div
                        class="text-base"
                        :class="data.change > 0 ? 'text-success' : 'text-error'"
                      >
                        ({{ data.change }}%)
                      </div>
                    </div>
                    <div class="text-sm">
                      {{ data.desc }}
                    </div>
                  </div>
                  <VAvatar
                    :color="data.iconColor"
                    variant="tonal"
                    rounded
                    size="42"
                  >
                    <VIcon
                      :icon="data.icon"
                      size="26"
                    />
                  </VAvatar>
                </div>
              </VCardText>
            </VCard>
          </VCol>
        </template>
      </VRow>
    </div>
    <VCard class="mb-6">
      <VCardItem class="pb-4">
        <VCardTitle>Хайх</VCardTitle>
      </VCardItem>

      <VCardText>
        <VRow>
          <VCol
            cols="6"
            sm="4"
          >
            <AppSelect
              v-model="selectedbadge"
              placeholder="Badge"
              :items="badge"
              clearable
              clear-icon="tabler-x"
            />
          </VCol>
          <VCol
            cols="6"
            sm="4"
          >
            <AppSelect
              v-model="selectedRole"
              placeholder="Rank"
              :items="role"
              clearable
              clear-icon="tabler-x"
            />
          </VCol>
    
          <VCol
            cols="6"
            sm="4"
            >
            <AppTextField
              v-model="searchQuery"
              placeholder="Тоглогч хайх"
            />
            </VCol>
        </VRow>
      </VCardText>
    </VCard>
    <VRow>
      <VCol
        v-for="(player, index) in filteredPlayers"
        :key="index"
        cols="12"
        lg="4"
        md="6"
      >
        <VCard>
          <VCardTitle class="d-flex align-center justify-space-between">
            <div class="d-flex align-center">
              <VAvatar class="me-4" size="84" style="border: 4px solid #374151; border-radius: 50%;">
                <template v-if="player.avatar">
                <VImg :src="player.avatar" />
            </template>
            <template v-else>
                <span class="text-h5 text-uppercase" :class="isDark ? 'text-white' : 'text-black'">
                {{ player.name.charAt(0) }}
                </span>
            </template>
              </VAvatar>
              <div>
                <div class="text-h4" :class="isDark ? 'text-white' : 'text-black'">{{ player.name }}</div>
                <div class="d-flex align-center mt-1 text-body-1">
                  <VIcon icon="mdi-star-circle-outline" class="me-1" />
                  • {{ player.role }} 
                  <VIcon icon="mdi-steam" class="ms-4 me-1" />
                  • {{ player.platform }}
                </div>
              </div>
            </div>
          </VCardTitle>
  
          <VCardText>
            <div class="d-flex flex-wrap mt-1 mb-3">
                <VChip
                  v-for="(badgeLabel, badgeIndex) in player.badge"
                  :key="badgeIndex"
                  :color="getRandomColor()"
                  class="me-2 mb-1"
                >
                  <span class="text-caption">{{ badgeLabel }}</span>
                </VChip>
              </div>
              <div class="d-flex flex-wrap mb-3">
                <VCard
                    class="me-3 mb-2"
                    elevation="1"
                    :style="{ width: '194px', height: '80px', backgroundColor: isDark ? '#374151' : '#f4f4f5' }"
                >
                    <VCardTitle class="text-subtitle-2 font-weight-bold">PUBG ID</VCardTitle>
                    <VCardText :class="isDark ? 'text-white' : 'text-black'" class="pt-0">
                    {{ player.pubg_id }}
                    </VCardText>
                </VCard>

                <VCard
                    class="mb-2"
                    elevation="1"
                    :style="{ width: '194px', height: '80px', backgroundColor: isDark ? '#374151' : '#f4f4f5' }"
                >
                    <VCardTitle class="text-subtitle-2 font-weight-bold">Туршлага</VCardTitle>
                    <VCardText :class="isDark ? 'text-white' : 'text-black'" class="pt-0">
                    {{ player.experience }}
                    </VCardText>
                </VCard>
                </div>

                <VCard
                class="mb-2"
                elevation="1"
                :style="{ backgroundColor: isDark ? '#374151' : '#f4f4f5' }"
                >
                <VCardTitle class="text-subtitle-2 font-weight-bold">BIO</VCardTitle>
                <VCardText :class="isDark ? 'text-white' : 'text-black'" class="pt-0">
                    {{ player.bio }}
                </VCardText>
                </VCard>
          </VCardText>
  
          <VBtn block class="rounded-t-0" @click="openDialog(player)">
        Профайл Үзэх
        </VBtn>
        </VCard>
      </VCol>
    </VRow>
    <VDialog v-model="isDialogOpen" max-width="1200">
  <DialogCloseBtn @click="isDialogOpen = false"/>
  <div class="d-flex flex-wrap>">
      <div class="d-p-flex flex-column pa-0">
  <VCard class="mb-4">
    <VCardTitle class="d-flex justify-space-between align-center mt-3 font-weight-bold">
      Дэлгэрэнгүй мэдээлэл
    </VCardTitle>

    <VCardText>
      <div class="d-flex align-center mb-4">
        <VAvatar size="134" class="me-4" style="border: 4px solid #374151; border-radius: 50%;">
          <template v-if="selectedPlayer?.avatar">
            <VImg :src="selectedPlayer.avatar" />
          </template>
          <template v-else>
            <span class="text-h5 text-uppercase" :class="isDark ? 'text-white' : 'text-black'">
              {{ selectedPlayer?.name.charAt(0) }}
            </span>
          </template>
        </VAvatar>
        <div>
          <span class="text-h1" :class="isDark ? 'text-white' : 'text-black'">{{ selectedPlayer?.name }}</span>
          <div class="d-flex align-center mt-1" style="font-size: 20px;">
            <VIcon icon="mdi-star-circle-outline" class="me-1" size="28" />
            • {{ selectedPlayer?.role }}
            <VIcon icon="mdi-steam" class="ms-4 me-1" size="28" />
            • {{ selectedPlayer?.platform }}
          </div>
        </div>
      </div>

      <div class="mb-3">
        <strong>BADGE:</strong>
        <div class="d-flex flex-wrap mt-2">
          <VChip
            v-for="(badge, index) in selectedPlayer?.badge"
            :key="index"
            :color="getRandomColor()"
            class="me-2 mb-1"
          >
            {{ badge }}
          </VChip>
        </div>
      </div>

      <div class="d-flex flex-wrap mb-3">
        <VCard
          class="me-3 mb-2"
          elevation="1"
          :style="{ width: '254px', height: '80px', backgroundColor: isDark ? '#374151' : '#f4f4f5' }"
        >
          <VCardTitle class="text-subtitle-2 font-weight-bold">PUBG ID</VCardTitle>
          <VCardText :class="isDark ? 'text-white' : 'text-black'" class="pt-0">
            {{ selectedPlayer?.pubg_id }}
          </VCardText>
        </VCard>

        <VCard
          class="mb-2"
          elevation="1"
          :style="{ width: '254px', height: '80px', backgroundColor: isDark ? '#374151' : '#f4f4f5' }"
        >
          <VCardTitle class="text-subtitle-2 font-weight-bold">Туршлага</VCardTitle>
          <VCardText :class="isDark ? 'text-white' : 'text-black'" class="pt-0">
            {{ selectedPlayer?.experience }}
          </VCardText>
        </VCard>
      </div>
    </VCardText>
  </VCard>

  <VCard>
    <VCardTitle class="font-weight-bold mt-3 ">
      BIO
    </VCardTitle>
    <VCardText class="mb-5" :class="[isDark ? 'text-white' : 'text-black']">
  <!-- Bio -->
  <div v-if="selectedPlayer?.bio">
    {{ selectedPlayer.bio }}
  </div>
  <div v-else class="text-medium-emphasis mt-2 text-center w-100">
    Энэ тоглогч BIO оруулаагүй байна.
  </div>
</VCardText>
  </VCard>


</div>
<div class="d-p-flex flex-column pa-0 ms-4" style="width: 50%; max-width: 100%;">

  <VCard class="mt-0" >
  <VCardTitle class="font-weight-bold mt-3" >
    Highlights
  </VCardTitle>
  <VCardText>
    <div v-if="selectedPlayer?.youtube_embed && selectedPlayer.youtube_embed.includes('<iframe')">
      <div class="video-container" v-html="selectedPlayer.youtube_embed"></div>
    </div>
    <div v-else class="d-flex flex-column align-center justify-center" max-height="320px" style="height: 320px; border-radius: 8px; background-color: rgba(0, 0, 0, 0.05);">
  <VIcon icon="mdi-video" size="88" />
  <div :class="[isDark ? 'text-white' : 'text-black']">
    Энэ тоглогч Highlight оруулаагүй байна.
  </div>
</div>
  </VCardText>
</VCard>
<VCard class="mt-4">
  <VCardTitle class="font-weight-bold mt-3">
    Social Links
  </VCardTitle>

  <VCardText :class="[isDark ? 'text-white' : 'text-black']">
  <div v-if="selectedPlayer?.social_links" class="d-flex flex-wrap gap-3 mt-1">
    <VBtn
      v-if="selectedPlayer.social_links.facebook"
      :href="selectedPlayer.social_links.facebook"
      target="_blank"
      rel="noopener"
      color="primary"
      variant="outlined"
      prepend-icon="mdi-facebook"
    >
      Facebook
    </VBtn>
    <VBtn
      v-if="selectedPlayer.social_links.instagram"
      :href="selectedPlayer.social_links.instagram"
      target="_blank"
      rel="noopener"
      color="error"
      variant="outlined"
      prepend-icon="mdi-instagram"
    >
      Instagram
    </VBtn>
    <VBtn
      v-if="selectedPlayer.social_links.steam"
      :href="selectedPlayer.social_links.steam"
      target="_blank"
      rel="noopener"
      color="default"
      variant="outlined"
      prepend-icon="mdi-steam"
    >
      Steam
    </VBtn>
    <VBtn
      v-if="selectedPlayer.social_links.discord"
      :href="selectedPlayer.social_links.discord"
      target="_blank"
      rel="noopener"
      color="secondary"
      variant="outlined"
      prepend-icon="mdi-discord"
    >
      Discord
    </VBtn>
    <VBtn
      v-if="selectedPlayer.social_links.twitch"
      :href="selectedPlayer.social_links.twitch"
      target="_blank"
      rel="noopener"
      color="success"
      variant="outlined"
      prepend-icon="mdi-twitch"
    >
      Twitch
    </VBtn>
  </div>

  <div v-else :class="isDark ? 'text-white' : 'text-black'" class="text-medium-emphasis mt-2 mb-3 text-center w-100">
    Энэ тоглогч сошиал хаяг оруулаагүй байна.
  </div>
</VCardText>
</VCard>
</div>
</div>

</VDialog>

  </template>
  
  <script setup>
  import avatar3 from '@images/avatars/abdie.jpg'
import avatar1 from '@images/avatars/pubg.png'
import avatar2 from '@images/avatars/rulka.jpg'
import { useTheme } from 'vuetify'



const theme = useTheme()
const isDark = computed(() => theme.global.name.value === 'dark')

const colors = ['primary', 'success', 'info', 'warning', 'error', 'secondary']
const isDialogOpen = ref(false)
const selectedPlayer = ref(null)

const openDialog = (player) => {
  selectedPlayer.value = player
  isDialogOpen.value = true
}
const searchQuery = ref('')
const selectedbadge = ref()
const selectedRole = ref()
const filteredPlayers = computed(() => {
  return players.filter(player => {
    const matchesSearch = !searchQuery.value || player.name.toLowerCase().includes(searchQuery.value.toLowerCase())

    const matchesRole = !selectedRole.value || player.role === selectedRole.value

    const matchesBadge = !selectedbadge.value || Object.values(player.badge).includes(selectedbadge.value)

    return matchesSearch && matchesRole && matchesBadge
  })
})
const getRandomColor = () => {
  return colors[Math.floor(Math.random() * colors.length)]
}
  const players = [
    {
      id: 0,
      name: 'SANTANA',
      avatar: avatar1,
      role: 'Tier-5',
      pubg_id: 'TNT_SANTANA',
      social_links: {
        facebook: 'https://facebook.com/santana',
        instagram: 'https://instagram.com/santana',
        twitch: 'https://twitch.com/santana',
      },
      badge: {
        0: 'хүндэт PUBG-чин',
        1: 'MEMBER'
      },
      experience: '5 жил',
      platform: 'Steam',
      bio: '123.',
      description: 'Specializes in stealth tactics and long-range combat.'
    },
    {
      id: 1,
      name: 'Rullet',
      avatar: avatar2,
      social_links: null,
      youtube_embed:'<iframe width="560" height="315" src="https://www.youtube.com/embed/TWUKUQtrnRU?si=yendAidfJ96CXTsH" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>',
      badge: {
        0: 'хүндэт PUBG-чин',
      },
      role: 'Tier-4',
      experience: '2 жил',
      pubg_id: 'Ru11et',
      platform: 'Steam',
      bio: ' Team bulaa ehellee bagaa olooroi.',
      description: 'Dominates in close-quarter combat with quick reflexes.'
    },
    {
      id: 2,
      name: 'Abdie',
      avatar: avatar3,
      social_links: {
        facebook: 'https://facebook.com/abdie',
        steam: 'https://steam.com/abdie',
        discord: 'https://discord.com/invite/abdie',
        twitch: 'https://twitch.com/santana',
      },
      role: 'Tier 7-2',
      youtube_embed: '<iframe width="560" height="315" src="https://www.youtube.com/embed/2zquaQQDz5c?si=OTewCmRo0EmqRmcH" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>',
      experience: '6 жил',
      pubg_id: 'Abdieeeee',
      badge: {
        0: 'VIP MEMBER'
      },
      platform: 'Steam',
      bio: 'Zaa chi aimaraaaa.',
      description: 'Always thinks two steps ahead on the battlefield.'
    }
    
  ]
  const widgetData = ref([
  {
    title: 'Нийт тоглогч',
    value: '359',
    change: 29,
    icon: 'tabler-users',
    iconColor: 'primary',
  },
  {
    title: 'Шинээр нэмэгдсэн',
    value: '67',
    change: 18,
    icon: 'tabler-user-plus',
    iconColor: 'warning',
  },
  {
    title: 'Идвэхтэй тоглогч',
    value: '110',
    change: -14,
    icon: 'tabler-user-check',
    iconColor: 'success',
  },
  {
    title: 'Идвэхгүй тоглогч',
    value: '57',
    change: 2,
    icon: 'tabler-user-search',
    iconColor: 'error',
  },
])
const badge = computed(() => {
  const badgeSet = new Set()
  players.forEach(player => {
    Object.values(player.badge).forEach(b => badgeSet.add(b))
  })
  return Array.from(badgeSet)
})
const role = computed(() => {
  const roleSet = new Set(players.map(player => player.role))
  return Array.from(roleSet)
})
  </script>
  
  <style scoped>
.video-container {
  position: relative;
  padding-bottom: 56.25%; 
  height: 320px;
  border-radius: 8px;
}

.video-container iframe {
  position: absolute;
  top: 0;
  left: 0;
  width: 50px !important; /* Responsive width */
  height: 100% !important;
  border: none;
  max-height: 300px; /* Smaller height */
  max-width: 600px;
}

</style>
