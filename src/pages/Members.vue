<template>
  <section class="presence-container--flex">
    <MemberCard v-for="member in members" :key="member.id"
                :member="member" :editable="canEdit"
                @delete-member="deleteMember" />
    <MdcFAB v-if="canEdit" icon-name="add" :link="{name: 'member:new'}"
            class="presence-fab__bottom-right--fixed" />
  </section>
</template>

<script lang="ts">
import { computed, defineComponent } from 'vue';
import { useStore } from 'vuex';
import { MdcFAB, MemberCard } from '../components';
import { Member } from '../types';

export default defineComponent({
  components: {
    MemberCard,
    MdcFAB
  },
  setup() {
    const store = useStore();

    const members = computed(() => store.getters['member/members'] as Member[]);

    const deleteMember = (member: Member) => {
      if (confirm(`Delete user "${member.lastName}, ${member.firstName}"?`)) {
        store.dispatch('member/deleteMember', member.id);
      }
    };

    const canEdit = computed(() => store.getters['auth/isAdmin'] as boolean);

    return {
      members,
      deleteMember,
      canEdit
    };
  }
});
</script>

<style lang="scss" scoped>
@use "../assets/style/presence-container";
@use "../assets/style/presence-ui";
</style>
