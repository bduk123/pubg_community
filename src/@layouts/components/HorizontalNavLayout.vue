<script setup>
import { HorizontalNav } from '@layouts/components'
import { themeConfig } from '@themeConfig'
// import { useLayouts } from '@layouts'
import NavbarThemeSwitcher from '@/layouts/components/NavbarThemeSwitcher.vue'
import UserProfile from '@/layouts/components/UserProfile.vue'
import { useLayouts } from '@layouts/composable/useLayouts'
const props = defineProps({
  navItems: {
    type: null,
    required: true,
  },
})

const { y: windowScrollY } = useWindowScroll()
const { width: windowWidth } = useWindowSize()
const router = useRouter()
const shallShowPageLoading = ref(false)
const isIndexRoute = computed(() => router.currentRoute.value.path === '/')
router.beforeEach(() => {
  shallShowPageLoading.value = true
})
router.afterEach(() => {
  shallShowPageLoading.value = false
})

const {
  _layoutClasses: layoutClasses,
  isNavbarBlurEnabled,
} = useLayouts()
</script>

<template>
  <div
    class="layout-wrapper"
    :class="layoutClasses(windowWidth, windowScrollY)"
  >
    <div
      class="layout-navbar-and-nav-container"
      :class="isNavbarBlurEnabled && 'header-blur'"
    >
      <!-- 👉 Navbar -->
      <!-- <div class="layout-navbar">
        <div class="navbar-content-container">
          <slot name="navbar" />
        </div>
      </div> -->
      <!-- 👉 Navigation -->
      <div class="layout-horizontal-nav">
        
        <div class="horizontal-nav-content-container d-flex justify-between align-items-end w-100">
        <!-- Left Side -->
        <div class="d-flex align-items-end gap-4 flex-wrap">
          <h1 class="app-title font-weight-bold leading-normal text-xl text-capitalize mb-2">
            {{ themeConfig.app.title }}
          </h1>
        </div>
        <div class="d-flex justify-center flex-grow-1">
          <HorizontalNav :nav-items="navItems" />
        </div>
        <!-- Right Side -->
        <div class="d-flex align-items-end gap-2">
          <NavbarThemeSwitcher />
          <UserProfile />
        </div>
      </div>
      </div>
    </div>

    <template v-if="!isIndexRoute">
      <main class="layout-page-content">
        <template v-if="$slots['content-loading']">
          <template v-if="shallShowPageLoading">
            <slot name="content-loading" />
          </template>
          <template v-else>
            <slot />
          </template>
        </template>
        <template v-else>
          <slot />
        </template>
      </main>
    </template>

    <template v-else>
      
      <slot />
    </template>


    <!-- 👉 Footer -->
    <footer class="layout-footer">
      <div class="footer-content-container">
        <slot name="footer" />
      </div>
    </footer>
  </div>
</template>

<style lang="scss">
@use "@configured-variables" as variables;
@use "@layouts/styles/placeholders";
@use "@layouts/styles/mixins";
.horizontal-nav-content-container {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  width: 100%;

  .app-title {
    margin-bottom: 0;
  }

  > div {
    display: flex;
    align-items: flex-end;
    gap: 1rem;
    flex-wrap: wrap;
  }
}
.layout-wrapper {
  &.layout-nav-type-horizontal {
    display: flex;
    flex-direction: column;

    // // TODO(v2): Check why we need height in vertical nav & min-height in horizontal nav
    // min-height: 100%;
    min-block-size: calc(var(--vh, 1vh) * 100);

    .layout-navbar-and-nav-container {
      z-index: 1;
    }

    .layout-navbar {
      z-index: variables.$layout-horizontal-nav-layout-navbar-z-index;
      block-size: variables.$layout-horizontal-nav-navbar-height;

      // ℹ️ For now we are not independently managing navbar and horizontal nav so we won't use below style to avoid conflicting with combo style of navbar and horizontal nav
      // If we add independent style of navbar & horizontal nav then we have to add :not for avoiding conflict with combo styles
      // .layout-navbar-sticky & {
      //   @extend %layout-navbar-sticky;
      // }

      // ℹ️ For now we are not independently managing navbar and horizontal nav so we won't use below style to avoid conflicting with combo style of navbar and horizontal nav
      // If we add independent style of navbar & horizontal nav then we have to add :not for avoiding conflict with combo styles
      // .layout-navbar-hidden & {
      //   @extend %layout-navbar-hidden;
      // }
    }

    // 👉 Navbar
    .navbar-content-container {
      @include mixins.boxed-content;
    }

    // 👉   Content height fixed
    &.layout-content-height-fixed {
      max-block-size: calc(var(--vh) * 100);

      .layout-page-content {
        overflow: hidden;

        > :first-child {
          max-block-size: 100%;
          overflow-y: auto;
        }
      }
    }

    // 👉 Footer
    // Boxed content
    .layout-footer {
      .footer-content-container {
        @include mixins.boxed-content;
      }
    }
  }

  // If both navbar & horizontal nav sticky
  &.layout-navbar-sticky.horizontal-nav-sticky {
    .layout-navbar-and-nav-container {
      position: sticky;
      inset-block-start: 0;
      will-change: transform;
    }
  }

  &.layout-navbar-hidden.horizontal-nav-hidden {
    .layout-navbar-and-nav-container {
      display: none;
    }
  }
}

// 👉 Horizontal nav nav
.layout-horizontal-nav {
  z-index: variables.$layout-horizontal-nav-z-index;

  // .horizontal-nav-sticky & {
  //   width: 100%;
  //   will-change: transform;
  //   position: sticky;
  //   top: 0;
  // }

  // .horizontal-nav-hidden & {
  //   display: none;
  // }

  .horizontal-nav-content-container {
    @include mixins.boxed-content(true);
  }
}
</style>
