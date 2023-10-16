<template>
  <SettingsPage page-title="settings.users.users">
    <template #actions>
      <VBtn
        color="primary"
        variant="elevated"
        class="ml-a"
        @click="$router.push('/settings/users/new')">
        {{ t('settings.users.new') }}
      </VBtn>
      <VBtn
        variant="elevated"
        href="https://jellyfin.org/docs/general/server/users/adding-managing-users"
        rel="noreferrer noopener"
        target="_blank">
        {{ t('settings.help') }}
      </VBtn>
    </template>
    <template #content>
      <VRow>
        <VCard
          v-for="user in users"
          :key="user.Id"
          class="mx-2"
          width="256px"
          @click="$router.push(`/settings/users/${user['Id']}`)">
          <VAvatar
            size="256"
            :rounded="false">
            <VImg
              :src="`${remote.sdk.api?.basePath}/Users/${user['Id']}/Images/Primary?height=256&tag=${user['PrimaryImageTag']}&quality=90`"
              width="256"
              height="256"
              cover>
              <template #placeholder>
                <VAvatar
                  :rounded="false"
                  color="primary"
                  size="256">
                  <VIcon size="256">
                    <IMdiAccount />
                  </VIcon>
                </VAvatar>
              </template>
            </VImg>
          </VAvatar>
          <VCardTitle>
            {{ user.Name }}
          </VCardTitle>
          <VCardSubtitle>
            {{ $t('settings.users.lastActivityDate', {
              value: user.LastActivityDate ? getRelativeTime(new Date(user.LastActivityDate)) : t('unknown')})
            }}
          </VCardSubtitle>
        </VCard>
      </VRow>
    </template>
  </SettingsPage>
</template>

<route lang="yaml">
meta:
  admin: true
</route>

<script setup lang="ts">
import { getUserApi } from '@jellyfin/sdk/lib/utils/api/user-api';
import { ref } from 'vue';
import { useI18n } from 'vue-i18n';
import { useRemote } from '@/composables';
import {getRelativeTime} from '@/utils/time';

const { t } = useI18n();
const remote = useRemote();

const users = ref(
  (await remote.sdk.newUserApi(getUserApi).getUsers()).data ?? []
);
</script>
