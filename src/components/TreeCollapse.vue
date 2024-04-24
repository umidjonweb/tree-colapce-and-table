<template>
   <div class="tree-menu">
      <div :style="{ 'padding-left': `${depth * 16}px` }" @click="_visible = !_visible" class="list-item">
         <div class="icon">
            <img v-if="item.child" :class="{ 'rotate-90': _visible }" class="arrow" src="@/assets/img/arrow.svg" alt="">
         </div>
         <p class="title">{{ item.title }}</p>
      </div>
      <div v-if="_visible">
         <template v-if="item?.child?.length">
            <TreeCollapse v-for="childList in item.child" :key="childList.id" :item="childList" :depth="depth + 1" />
         </template>
      </div>
   </div>
</template>

<script setup lang="ts">
import { defineProps, ref } from "vue";
interface Props {
   item: {
      title: string;
      id: number;
      child?: {
         title: string;
         id: number;
      }[];
   };
   depth: number;
   titleClass?: string;
}
defineProps<Props>();

const _visible = ref(false);

</script>

<style scoped lang="scss">
.tree-menu {
   background: #f2efefe8;
   cursor: pointer;
   .title {
      white-space: nowrap;
      font-size: 14px;
   }
   
   .list-item {
      padding: 5px 50px 5px 9px;

      display: flex;
      gap: 8px;
      align-items: center;
      border-left: 4px solid #f2efefe8;
      &:hover{
         border-left: 4px solid blue;
         background: #ccc;
      }
   }

   .hide-list {
      opacity: 0;
      transition: all 0.3s ease-in;
      overflow: hidden;
      height: 0;
   }

   .animation {
      opacity: 1;
      height: auto;
   }


   .icon {
      width: 20px;
      height: 20px;
      display: grid;
      place-items: center;

      .arrow {
         transition: all 0.3s ease;
         font-weight: bold;
         color: limegreen;
      }
   }

   .rotate-90 {
      transform: rotate(90deg);
      transition: all 0.3s ease;
   }
}

.fade-enter-active,
.fade-leave-active {
   transform: translateY(0);
   transition: all 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
   transform: translateY(-20px);
   opacity: 0;
}
</style>
