<template>
  <NoticeComponent />
  <NavigationComponent @toggleMobileNavigation="toggleMobileNavigation" />
  <MobileNavigationComponent
    :show="showMobileNavigation"
    @toggleMobileNavigation="toggleMobileNavigation" />

  <main>
    <div class="container" style="margin-bottom: 1.5rem">
      <AlertComponent>
        {{ $t('general.alert.development_notice') }}
        {{ $t('general.alert.encounter_issues') }}
        
        <a href="https://github.com/carlssonemil/nebula/issues/new?title=[Bug]%20" target="_blank"
          >{{ $t('general.alert.report_link_text') }}</a>. 
        
        {{ $t('general.alert.missing_images_notice') }} {{ $t('general.alert.thank_you') }}
      </AlertComponent>
    </div>

    <router-view v-slot="{ Component }">
      <transition name="page-fade" mode="out-in">
        <component :is="Component" />
      </transition>
    </router-view>
  </main>

  <notifications position="top center">
    <template #body="props">
      <div class="notification" :class="props.item.type" @click="props.close">
        <p class="title">{{ props.item.title }}</p>
        <IconComponent class="remove" name="times" width="18" height="18" />
      </div>
    </template>
  </notifications>

  <FooterComponent />
</template>

<script>
import { useStore } from '@/stores/store'

import FooterComponent from '@/components/layout/FooterComponent.vue'
import MobileNavigationComponent from '@/components/layout/MobileNavigationComponent.vue'
import NavigationComponent from '@/components/layout/NavigationComponent.vue'
import NoticeComponent from './components/NoticeComponent.vue'
import AlertComponent from '@/components/AlertComponent.vue'

export default {
  components: {
    FooterComponent,
    MobileNavigationComponent,
    NavigationComponent,
    NoticeComponent,
    AlertComponent,
  },

  data() {
    return {
      showMobileNavigation: false,
    }
  },

  async beforeCreate() {
    const store = useStore()
    await store.getStoredProgress()
  },

  watch: {
    $route() {
      this.showMobileNavigation = false
    },
  },

  methods: {
    toggleMobileNavigation() {
      this.showMobileNavigation = !this.showMobileNavigation
    },
  },
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap');
@import '@/scss/main';

#app {
  display: flex;
  flex-direction: column;
  height: 100%;
}
</style>
