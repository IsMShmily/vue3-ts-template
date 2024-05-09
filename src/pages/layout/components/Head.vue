<script setup lang="ts">
import { useTheme } from "vuetify";
import { Moon, Sun, EllipsisVertical } from "lucide-vue-next";
import { ref } from "vue";
import router from "@/router";
import { Github, UserRoundPlus, LogOut } from "lucide-vue-next";

const theme = useTheme();
const drawer = ref(false);
const menu = ref(false);
const userInfo = ref();

const items = [
  {
    title: "主页",
    value: "home",
    icon: "Home",
  },
  {
    title: "博客",
    value: "blogs",
    icon: "BookCheck",
  },
  {
    title: "留言",
    value: "leave",
    icon: "Send",
  },
  {
    title: "我的",
    value: "chat",
    icon: "Users",
  },
];
const jumpRouter = (item: any) => {
  menu.value = false;
  router.push(item.value);
};
const goOut = () => {};
/**
 * @description: 切换主题
 * @return {*}
 */
const toggleTheme = () => {
  theme.global.name.value = theme.global.current.value.dark ? "light" : "dark";
};

/**
 * 用户登录
 */
const LoginDialogstatus = ref(false);
const userLogin = () => {
  LoginDialogstatus.value = true;
};

/**
 * 注册
 */
const signInDialog = ref(false);
const openSignInDiaog = () => {
  signInDialog.value = true;
};
const signIn = () => {};
</script>
<template>
  <v-app-bar scroll-behavior="elevate">
    <v-toolbar
      color="rgba(255,255,255,0.001)"
      :dark="true"
      class="px-5 box-border"
    >
      <v-toolbar-title class="">
        <div class="flex items-center">
          <img src="/logo.png" class="rounded-full w-10" alt="" />
          <div class="ml-3 font-bold cursor-pointer">shmily_yy 博客</div>
        </div>
      </v-toolbar-title>

      <!-- <v-btn size="small" elevation="3" icon="">
        <Gitlab :size="17" />
      </v-btn>
      <v-btn size="small" elevation="3" icon="" class="ml-3">
        <Github :size="17" />
      </v-btn> -->

      <v-btn
        class="ml-3"
        @click="toggleTheme"
        size="small"
        elevation="3"
        rounded="xl"
        icon=""
      >
        <v-expand-x-transition>
          <Moon v-show="!theme.global.current.value.dark" :size="17" />
        </v-expand-x-transition>
        <v-expand-x-transition>
          <Sun v-show="theme.global.current.value.dark" :size="17" />
        </v-expand-x-transition>
      </v-btn>

      <v-menu v-model="menu" :close-on-content-click="false" location="top">
        <template v-slot:activator="{ props }">
          <v-btn
            size="small"
            elevation="3"
            icon=""
            class="ml-3"
            @click="drawer = true"
            v-bind="props"
          >
            <EllipsisVertical class="cursor-pointer" />
          </v-btn>
        </template>

        <!-- card -->
        <v-card min-width="280">
          <v-list>
            <v-list-item
              v-if="userInfo"
              prepend-avatar="https://cdn.vuetifyjs.com/images/john.jpg"
              subtitle="欢迎来到shmily_yy的博客"
              title="用户名称"
            >
              <template v-slot:append>
                <v-btn variant="text" @click="goOut">
                  <LogOut :size="16"
                /></v-btn>
              </template>
            </v-list-item>
            <v-list-item v-else class="text-center">
              <v-list-item-title>选择登录方式</v-list-item-title>
              <v-list-item-title class="mt-3">
                <v-btn-toggle variant="outlined">
                  <v-btn variant="text">
                    <div
                      class="text-xs flex justify-center items-center flex-col"
                    >
                      <Github :size="16" />
                      <div class="mt-1 scale-x-75 scale-y-75">github</div>
                    </div>
                  </v-btn>
                  <v-btn variant="text" @click="userLogin">
                    <div
                      class="text-xs flex justify-center items-center flex-col"
                    >
                      <UserRoundPlus :size="16" />
                      <div class="mt-1 scale-x-75 scale-y-75">已有用户</div>
                    </div>
                  </v-btn>
                </v-btn-toggle></v-list-item-title
              >
              <template v-slot:append> </template>
            </v-list-item>
          </v-list>

          <v-divider></v-divider>

          <v-list>
            <v-list-item
              v-for="(item, i) in items"
              :key="i"
              class="min-w-36 text-center cursor-pointer"
            >
              <v-list-item-title @click="jumpRouter(item)">{{
                item.title
              }}</v-list-item-title>
            </v-list-item>
          </v-list>
        </v-card>
        <!-- card -->
      </v-menu>
    </v-toolbar>
  </v-app-bar>

  <v-dialog v-model="LoginDialogstatus" max-width="500">
    <template v-slot:default="{ isActive }">
      <v-card rounded="lg">
        <v-card-title class="d-flex justify-space-between align-center">
          <div class="text-h5 text-medium-emphasis ps-2">登录您的账号</div>

          <v-btn
            icon="mdi-close"
            variant="text"
            @click="isActive.value = false"
          ></v-btn>
        </v-card-title>

        <v-card-text>
          <div class="text-medium-emphasis mb-4">请输入您的用户名和密码</div>
          <v-text-field hide-details="auto" label="邮箱"></v-text-field>
        </v-card-text>
        <v-card-text>
          <v-text-field hide-details="auto" label="密码"></v-text-field
        ></v-card-text>

        <v-divider class="mt-2"></v-divider>

        <v-card-actions class="my-2 d-flex justify-end">
          <v-btn
            class="text-none"
            rounded="xl"
            text="去注册"
            @click="openSignInDiaog"
          ></v-btn>

          <v-btn
            class="text-none"
            rounded="xl"
            text="登录"
            variant="flat"
            @click="LoginDialogstatus = false"
          ></v-btn>
        </v-card-actions>
      </v-card>

      <v-dialog v-model="signInDialog" max-width="500">
        <template v-slot:default="{ isActive }">
          <v-card rounded="lg">
            <v-card-title class="d-flex justify-space-between align-center">
              <div class="text-h5 text-medium-emphasis ps-2">注册您的账号</div>

              <v-btn
                icon="mdi-close"
                variant="text"
                @click="isActive.value = false"
              ></v-btn>
            </v-card-title>

            <v-card-text>
              <div class="text-medium-emphasis mb-4">
                请输入您的用户名和密码
              </div>
              <v-text-field hide-details="auto" label="邮箱"></v-text-field>
            </v-card-text>
            <v-card-text>
              <v-text-field hide-details="auto" label="密码"></v-text-field
            ></v-card-text>
            <v-card-text>
              <v-text-field
                hide-details="auto"
                label="再次输入密码"
              ></v-text-field
            ></v-card-text>

            <v-divider class="mt-2"></v-divider>

            <v-card-actions class="my-2 d-flex justify-end">
              <v-btn
                class="text-none"
                rounded="xl"
                text="注册"
                @click="signIn"
              ></v-btn>
            </v-card-actions>
          </v-card>
        </template>
      </v-dialog>
    </template>
  </v-dialog>
</template>
<style lang="scss" scoped>
.logo {
  width: 50px;
  height: 50px;
}

header {
  position: fixed !important;
}
</style>