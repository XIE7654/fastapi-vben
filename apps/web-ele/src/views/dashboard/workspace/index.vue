<script lang="ts" setup>
import type {
  WorkbenchProjectItem,
  WorkbenchQuickNavItem,
} from '@vben/common-ui';

import { ref } from 'vue';
import { useRouter } from 'vue-router';

import {
  WorkbenchHeader,
  WorkbenchProject,
  WorkbenchQuickNav,
} from '@vben/common-ui';
import { preferences } from '@vben/preferences';
import { useUserStore } from '@vben/stores';
import { openWindow } from '@vben/utils';

const userStore = useUserStore();

// 项目列表
const projectItems: WorkbenchProjectItem[] = [
  {
    color: '#009688',
    content: 'github.com/XIE7654/fastapi-admin',
    date: 'FastAPI',
    group: 'Python 后端管理系统',
    icon: 'simple-icons:fastapi',
    title: 'FastAPI Admin',
    url: 'https://github.com/XIE7654/fastapi-admin',
  },
  {
    color: '#e18525',
    content: 'github.com/XIE7654/fastapi-vben',
    date: 'Vue3 + Ant Design',
    group: 'Vben Admin 前端',
    icon: 'devicon:antdesign',
    title: 'FastAPI Vben',
    url: 'https://github.com/XIE7654/fastapi-vben',
  },
  {
    color: '#409EFF',
    content: 'github.com/XIE7654/fastapi-ele',
    date: 'Vue3 + Element Plus',
    group: 'Element Plus 前端',
    icon: 'ep:element-plus',
    title: 'FastAPI Element',
    url: 'https://github.com/XIE7654/fastapi-ele',
  },
  {
    color: '#092E20',
    content: 'github.com/XIE7654/django-vue3-admin',
    date: 'Django + Vue3',
    group: 'Django 管理系统',
    icon: 'simple-icons:django',
    title: 'Django Vue3 Admin',
    url: 'https://github.com/XIE7654/django-vue3-admin',
  },
];

// 快捷导航
const quickNavItems: WorkbenchQuickNavItem[] = [
  {
    color: '#1fdaca',
    icon: 'ion:home-outline',
    title: '首页',
    url: '/',
  },
  {
    color: '#409EFF',
    icon: 'ep:user',
    title: '用户管理',
    url: '/system/user',
  },
  {
    color: '#67C23A',
    icon: 'ep:setting',
    title: '系统管理',
    url: '/system/menu',
  },
  {
    color: '#E6A23C',
    icon: 'ep:document',
    title: '系统监控',
    url: '/system/operatelog',
  },
];

const router = useRouter();

function navTo(nav: WorkbenchProjectItem | WorkbenchQuickNavItem) {
  if (nav.url?.startsWith('http')) {
    openWindow(nav.url);
    return;
  }
  if (nav.url?.startsWith('/')) {
    router.push(nav.url).catch((error) => {
      console.error('Navigation failed:', error);
    });
  } else {
    console.warn(`Unknown URL for navigation item: ${nav.title} -> ${nav.url}`);
  }
}
</script>

<template>
  <div class="p-5">
    <WorkbenchHeader
      :avatar="userStore.userInfo?.avatar || preferences.app.defaultAvatar"
    >
      <template #title>
        早安, {{ userStore.userInfo?.nickname }}, 开始您一天的工作吧！
      </template>
      <template #description> 今日晴，20℃ - 32℃！ </template>
    </WorkbenchHeader>

    <div class="mt-5 flex flex-col lg:flex-row">
      <div class="mr-4 w-full lg:w-3/5">
        <WorkbenchProject :items="projectItems" title="项目地址" @click="navTo" />
      </div>
      <div class="w-full lg:w-2/5">
        <WorkbenchQuickNav
          :items="quickNavItems"
          class="mt-5 lg:mt-0"
          title="快捷导航"
          @click="navTo"
        />
      </div>
    </div>
  </div>
</template>