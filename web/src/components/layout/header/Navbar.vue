<template>
  <div class="flex shadow-lg dark:shadow-sm bg-lime-600 text-neutral-content px-2 md:px-8 py-0 h-10 dark:bg-dark-gray-900">
    <div class="flex text-color-on-accent items-center">
      <router-link :to="{ name: 'home' }" class="w-12 h-10 overflow-hidden">
        <img class="mt-1 w-10" src="../../../assets/logo.svg?url" />
        <!--<span class="absolute -bottom-4 text-xs">{{ version }}</span>-->
      </router-link>
      <router-link
        v-if="user"
        :to="{ name: 'repos' }"
        class="mx-4 hover:bg-lime-700 dark:hover:bg-gray-600 px-4 py-1 rounded-md"
      >
        <span class="flex md:hidden">{{ $t('repos') }}</span>
        <span class="hidden md:flex">{{ $t('repositories') }}</span>
      </router-link>
    </div>
    <div class="flex ml-auto items-center space-x-4 text-color-on-accent">
      <a
        :href="docsUrl"
        target="_blank"
        class="hover:bg-lime-700 dark:hover:bg-gray-600 px-4 py-1 rounded-md hidden md:flex"
        >{{ $t('docs') }}</a
      >
      <IconButton
        :icon="darkMode ? 'dark' : 'light'"
        class="!text-color-on-accent"
        @click="darkMode = !darkMode"
      />
      <router-link v-if="user" :to="{ name: 'user' }">
        <img v-if="user && user.avatar_url" class="w-8 rounded-full" :src="`${user.avatar_url}`" />
      </router-link>
      <Button v-else :text="$t('login')" @click="doLogin" />
      <ActiveBuilds v-if="user" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { useRoute } from 'vue-router';

import Button from '~/components/atomic/Button.vue';
import IconButton from '~/components/atomic/IconButton.vue';
import useAuthentication from '~/compositions/useAuthentication';
import useConfig from '~/compositions/useConfig';
import { useDarkMode } from '~/compositions/useDarkMode';

import ActiveBuilds from './ActiveBuilds.vue';

export default defineComponent({
  name: 'Navbar',

  components: { Button, ActiveBuilds, IconButton },

  setup() {
    const config = useConfig();
    const route = useRoute();
    const authentication = useAuthentication();
    const { darkMode } = useDarkMode();
    const docsUrl = window.WOODPECKER_DOCS;

    function doLogin() {
      authentication.authenticate(route.fullPath);
    }

    const version = config.version?.startsWith('next') ? 'next' : config.version;

    return { darkMode, user: authentication.user, doLogin, docsUrl, version };
  },
});
</script>
