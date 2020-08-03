<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="leftDrawerOpen = !leftDrawerOpen"
        />

        <q-toolbar-title>
<!--          Quasar App-->
        </q-toolbar-title>
        <div v-if="this.$q.cookies.has('user_token')">
          {{ this.$q.cookies.get('user_name') }}
        </div>
        <div v-else>
          <q-btn flat to="/login">登陆</q-btn>
        </div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      content-class="bg-grey-1"
    >
      <q-list>
        <q-item-label
          header
          class="text-grey-8"
        >
          番茄用户中心
        </q-item-label>
        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
// import { Cookies } from 'quasar'
import EssentialLink from 'components/EssentialLink.vue'

const linksData = [
  {
    title: '项目查询',
    caption: 'quasar.dev',
    icon: 'school',
    link: 'search'
  },
  {
    title: '项目收藏',
    caption: 'github.com/quasarframework',
    icon: 'code',
    link: 'https://github.com/quasarframework'
  },
  {
    title: '获取短信码',
    caption: 'chat.quasar.dev',
    icon: 'chat',
    link: 'getSMS'
  },
  {
    title: '获取语音码',
    caption: 'forum.quasar.dev',
    icon: 'record_voice_over',
    link: '#'
  },
  {
    title: '消费明细',
    caption: '@quasarframework',
    icon: 'rss_feed',
    link: 'orderDetail'
  },
  {
    title: '退款明细',
    caption: '@QuasarFramework',
    icon: 'public',
    link: '#'
  }
]

export default {
  name: 'MainLayout',
  components: { EssentialLink },
  data () {
    return {
      leftDrawerOpen: false,
      essentialLinks: linksData
    }
  }
}
</script>
