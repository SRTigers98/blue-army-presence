<template>
  <MdcCard>
    <section class="presence-container--flex">
      <h1>Blue Army Presence</h1>
      <img src="../assets/img/logo.png" alt="Blue Army Logo">
      <MdcButton :link="currentSeasonRoute" :disabled="!currentSeasonRoute"
                 class="presence-home__button">
        Current Season
      </MdcButton>
    </section>
    <section class="presence-home__version">
      <small>{{ appVersion }}</small>
    </section>
  </MdcCard>
</template>

<script lang="ts">
import { computed, defineComponent } from 'vue';
import { useStore } from 'vuex';
import { LocationAsRelativeRaw } from 'vue-router';
import { MdcButton, MdcCard } from "../components";

export default defineComponent({
  components: {
    MdcCard,
    MdcButton
  },
  setup() {
    const store = useStore();

    const appVersion = computed(() => `v${store.getters['version']}`);

    const currentSeasonRoute = computed(() => {
      const currentSeasonId = store.getters['season/currentSeasonId'] as string;
      return currentSeasonId ?
          { name: 'season:games', params: { seasonId: currentSeasonId } } as LocationAsRelativeRaw :
          null;
    });

    return {
      appVersion,
      currentSeasonRoute
    };
  }
});
</script>

<style lang="scss" scoped>
@use "../assets/style/presence-container";

.presence-home__button {
  margin: 1rem 0;
}

.presence-home__version {
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
}
</style>
