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
        <div class="q-px-lg p-pt-xl q-mb-md">
          <div class="text-h3">Todo</div>
          <div class="text-subtitle1">{{ today }}</div>
        </div>
        <q-img
          src="../statics/mountain.jpg"
          class="header-image absolute-top"
        />
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      :width="200"
      :breakpoint="600"
    >
      <q-scroll-area
        style="
          height: calc(100% - 150px);
          margin-top: 150px;
          border-right: 1px solid #ddd;
        "
      >
        <q-list padding>
          <q-item to="/" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="list" />
            </q-item-section>

            <q-item-section> ToDo's </q-item-section>
          </q-item>

          <!-- <q-item to="/Profile" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="accessibility" />
            </q-item-section>

            <q-item-section> Perfil </q-item-section>
          </q-item> -->

          <q-item to="/Help" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="help" />
            </q-item-section>

            <q-item-section> Ajuda </q-item-section>
          </q-item>
        </q-list>
      </q-scroll-area>

      <q-img
        class="absolute-top"
        src="../statics/mountain.jpg"
        style="height: 150px"
      >
        <div class="absolute-bottom bg-transdarparent">
          <q-avatar size="56px" class="q-mb-sm">
            <img v-if="user.avatar != ''" :src="user.avatar" />
            <img v-else src="https://cdn.quasar.dev/img/boy-avatar.png" />
          </q-avatar>
          <div class="text-weight-bold">{{ user.name }}</div>

          <!--  -->

          <Dialog @userChange="changeUser($event)" />
        </div>
      </q-img>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from "components/EssentialLink.vue";
import Dialog from "components/Dialog.vue";

const linksData = [
  {
    title: "Docs",
    caption: "quasar.dev",
    icon: "school",
    link: "https://quasar.dev",
  },
  {
    title: "Github",
    caption: "github.com/quasarframework",
    icon: "code",
    link: "https://github.com/quasarframework",
  },
  {
    title: "Discord Chat Channel",
    caption: "chat.quasar.dev",
    icon: "chat",
    link: "https://chat.quasar.dev",
  },
  {
    title: "Forum",
    caption: "forum.quasar.dev",
    icon: "record_voice_over",
    link: "https://forum.quasar.dev",
  },
  {
    title: "Twitter",
    caption: "@quasarframework",
    icon: "rss_feed",
    link: "https://twitter.quasar.dev",
  },
  {
    title: "Facebook",
    caption: "@QuasarFramework",
    icon: "public",
    link: "https://facebook.quasar.dev",
  },
  {
    title: "Quasar Awesome",
    caption: "Community Quasar projects",
    icon: "favorite",
    link: "https://awesome.quasar.dev",
  },
];

import { date } from "quasar";

export default {
  name: "MainLayout",
  components: { EssentialLink, Dialog },
  data() {
    return {
      leftDrawerOpen: false,
      essentialLinks: linksData,
      user: {
        avatar: "",
        name: "Liam Cabral",
      },
    };
  },
  computed: {
    today() {
      let timeStamp = Date.now();
      return date.formatDate(timeStamp, "dddd D MMMM");
    },
  },
  methods: {
    changeUser(e) {
      this.user.avatar = e.avatar;
      this.user.name = e.name;
    },
    load() {
      // let picAvatar = this.$q.localStorage.getItem("avatar");
      // if (picAvatar == null) {
      //   return;
      // }
      let pickUser = this.$q.localStorage.getItem("user");

      this.user.avatar = pickUser.avatar;
      this.user.name = pickUser.name;
    },
  },
  created() {
    this.load();
  },
};
</script>

<style lang="css">
.header-image {
  height: 100%;
  z-index: -1;
  /* opacity: 0.2; */
  filter: grayscale(100%);
}
</style>