<template>
  <section class="app-main">
    <router-view v-slot="{ Component, route }">
      <transition name="fade-transform" mode="out-in">
        <keep-alive :include="tagsViewStore.cachedViews">
          <component v-if="!route.meta.link" :is="Component" :key="route.path"/>
        </keep-alive>
      </transition>
    </router-view>
    <iframe-toggle />
  </section>
</template>

<script setup>
import iframeToggle from "./IframeToggle/index"
import useTagsViewStore from '@/store/modules/tagsView'

const route = useRoute()
const tagsViewStore = useTagsViewStore()

onMounted(() => {
  addIframe()
})

watch((route) => {
  addIframe()
})

function addIframe() {
  if (route.meta.link) {
    useTagsViewStore().addIframeView(route)
  }
}
</script>

<style lang="scss" scoped>
@import "@/assets/styles/variables.module.scss";

.app-main {
  /* 75 = navbar height */
  min-height: calc(100vh - $navbar-height);
  width: 100%;
  position: relative;
  overflow: hidden;
  padding: 20px;
}

.fixed-header + .app-main {
  padding-top: $navbar-height;
}

.hasTagsView {
  .app-main {
    /* 109 = navbar + tags-view = $navbar-height + 34 */
    min-height: calc(100vh - $navbar-height - 34px);
  }

  .fixed-header + .app-main {
    padding-top: calc($navbar-height + 34px);
  }
}

//override root app container
:deep(.app-container) {
  background-color: #ffffff !important;
  padding: 20px;
  border-radius: 16px !important;
  min-height: calc(100vh - 75px - 40px) !important;
}

</style>

<style lang="scss">
// fix css style bug in open el-dialog
.el-popup-parent--hidden {
  .fixed-header {
    padding-right: 6px;
  }
}

::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}

::-webkit-scrollbar-track {
  background-color: #f1f1f1;
}

::-webkit-scrollbar-thumb {
  background-color: #c0c0c0;
  border-radius: 3px;
}
</style>

