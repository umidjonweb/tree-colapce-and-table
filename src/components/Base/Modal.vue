<template>
   <div class="modal" v-if="_visible">
      <div class="modal-item">
         <div class="modal-content">
            <span class="close" @click="_visible = false">&times;</span>
            <slot></slot>
         </div>
      </div>
   </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const _visible = ref(false)
const emit = defineEmits(['close'])

function open(isOpen = true) {
    console.log('asdasd');
   if (isOpen) {
      _visible.value = true
   }
   else {
      _visible.value = false
    }
}
function closeModal() {
   emit('close');
}

defineExpose({
   open
})
</script>

<style>
.modal {
   /* display: none; */
   position: fixed;
   z-index: 999;
   inset: 0;
   width: 100vw;
   height: 100%;
   overflow: auto;
   background-color: rgba(0, 0, 0, 0.4);
}

.modal-item{
   width: 100vw;
   height: 100%;
   display: flex;
   justify-content: space-between;
   align-items: center;
}
.modal-content {
   background-color: #fefefe;
   margin: auto;
   border-radius: 16px;
   padding: 24px;
   border: 1px solid #888;
   width: 100%;
   max-width: 700px;
}

.close {
   color: #aaa;
   float: right;
   font-size: 28px;
   font-weight: bold;
}

.close:hover,
.close:focus {
   color: black;
   text-decoration: none;
   cursor: pointer;
}
</style>