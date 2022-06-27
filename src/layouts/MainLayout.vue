<template>
  <div>
    <q-layout view="lHh Lpr lff" container style="height: 100vh">
      <q-header elevated class="bg-grey-10">
        <q-toolbar>
          <q-toolbar-title class="text-uppercase">
			<router-link to="/" class="text-white">test-quasar</router-link>
		  </q-toolbar-title>
          <q-btn flat @click="drawer = !drawer" round dense icon="menu" />
        </q-toolbar>
      </q-header>

      <q-drawer v-model="drawer" show-if-above :width="300" :breakpoint="400">
        <q-scroll-area
          style="
            height: calc(100% - 250px);
            margin-top: 250px;
            border-right: 1px solid #ddd;
          "
        >
          <q-list padding>
            <q-item
              clickable
              v-ripple
              tag="a"
              target="_blank"
              href="https://github.com/chunaixxx"
            >
              <q-item-section avatar>
                <q-icon
                  name="img:https://img.icons8.com/small/512/github.png"
                />
              </q-item-section>

              <q-item-section>GitHub</q-item-section>
            </q-item>

            <q-item
              clickable
              v-ripple
              tag="a"
              target="_blank"
              href="https://t.me/chunaichunai"
            >
              <q-item-section avatar>
                <q-icon name="telegram" />
              </q-item-section>

              <q-item-section>Telegram</q-item-section>
            </q-item>

            <q-item
              clickable
              v-ripple
              tag="a"
              target="_blank"
              href="mailto:chunaixxx@gmail.com"
            >
              <q-item-section avatar>
                <q-icon name="email" />
              </q-item-section>

              <q-item-section>Email</q-item-section>
            </q-item>
          </q-list>
        </q-scroll-area>

        <q-img
          class="absolute-top"
          src="https://avatars.githubusercontent.com/u/56599229?v=4"
          style="height: 250px"
        >
          <div class="absolute-bottom">
            <div class="text-weight-bold">Даниил Кириллов</div>
            <div>@chunaichunai</div>
          </div>
        </q-img>
      </q-drawer>

      <q-page-container>
        <q-page>
          <router-view />
        </q-page>
      </q-page-container>
    </q-layout>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const drawer = ref(false)
</script>
