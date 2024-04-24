<template>
   <div class="el-select el-select--multiple">
     <div class="el-input el-input--suffix">
       <div class="el-input__inner">
         <span v-if="selectedItems.length === 0" class="el-input__placeholder">{{ placeholder }}</span>
         <div v-else class="el-input__value">
           <span v-for="(item, index) in selectedItems" :key="index" class="el-tag el-tag--info el-tag--mini" @click="removeItem(item)">
             {{ item }} <img  src="@/assets/img/close.svg" />
           </span>
         </div>
         <input
           class="el-input__inner"
           :class="{ 'is-focus': isDropdownVisible }"
           v-model="searchText"
           @input="filterOptions"
           @focus="showDropdown"
           @blur="hideDropdown"
         />
       </div>
       <i class="el-input__suffix el-icon-arrow-up" :class="{ 'is-reverse': isDropdownVisible }" @click="toggleDropdown">
         <img src="@/assets/img/arrow.svg" />
      </i>
     </div>
     <transition name="el-zoom-in-top">
       <div v-if="isDropdownVisible" class="el-select-dropdown el-popper">
         <div class="el-scrollbar">
           <ul class="el-select-dropdown__list">
             <li
               v-for="(option, index) in filteredOptions"
               :key="index"
               class="el-select-dropdown__item"
               @click="toggleSelection(option)"
             >
               {{ option }}
             </li>
           </ul>
         </div>
       </div>
     </transition>
   </div>
 </template>
 
 <script>
 import { ref, computed } from 'vue';
 
 export default {
   props: {
     options: {
       type: Array,
       required: true
     },
     placeholder: {
       type: String,
       default: 'Select'
     }
   },
   setup(props) {
     const selectedItems = ref([]);
     const searchText = ref('');
     const isDropdownVisible = ref(false);
 
     const filteredOptions = computed(() => {
       return props.options.filter(option =>
         option.toLowerCase().includes(searchText.value.toLowerCase())
       );
     });
 
     const toggleSelection = (option) => {
       if (!selectedItems.value.includes(option)) {
         selectedItems.value.push(option);
       } else {
         const index = selectedItems.value.indexOf(option);
         selectedItems.value.splice(index, 1);
       }
     };
 
     const removeItem = (item) => {
       const index = selectedItems.value.indexOf(item);
       selectedItems.value.splice(index, 1);
     };
 
     const filterOptions = () => {
       isDropdownVisible.value = true;
     };
 
     const showDropdown = () => {
       isDropdownVisible.value = true;
     };
 
     const hideDropdown = () => {
       setTimeout(() => {
         isDropdownVisible.value = false;
       }, 200);
     };
 
     const toggleDropdown = () => {
       isDropdownVisible.value = !isDropdownVisible.value;
     };
 
     return {
       selectedItems,
       searchText,
       isDropdownVisible,
       filteredOptions,
       toggleSelection,
       removeItem,
       filterOptions,
       showDropdown,
       hideDropdown,
       toggleDropdown
     };
   }
 };
 </script>
 

 <style>
 /* Styles to mimic Element Plus multiselect */
 .el-select {
   width: 100%;
 }
 
 .el-input {
   display: flex;
   align-items: center;
   justify-content: space-between;
 }
 
 .el-input__inner {
   flex: 1;
 }
 
 .el-input__placeholder {
   color: #c0c4cc;
 }
 
 .el-tag {
   margin-right: 4px;
 }
 
 .el-tag__close {
   cursor: pointer;
 }
 
 .el-icon-arrow-up.is-reverse {
   transform: scaleY(-1);
 }
 
 .el-select-dropdown {
   position: absolute;
   z-index: 1000;
   max-height: 200px;
   overflow-y: auto;
   background-color: #fff;
   border: 1px solid #dcdfe6;
   border-top: none;
   border-radius: 4px;
 }
 
 .el-select-dropdown__list {
   padding: 8px;
 }
 
 .el-select-dropdown__item {
   padding: 4px 8px;
   cursor: pointer;
 }
 
 .el-select-dropdown__item:hover {
   background-color: #f5f7fa;
 }
 
 .el-popper {
   margin-top: 4px;
 }
 
 /* Transition for dropdown */
 .el-zoom-in-top-enter-active,
 .el-zoom-in-top-leave-active {
   transition: transform 0.3s cubic-bezier(0.55, 0.085, 0.68, 0.53);
 }
 
 .el-zoom-in-top-enter,
 .el-zoom-in-top-leave-to {
   transform: translateY(-20px);
   opacity: 0;
 }
 </style>
 
 