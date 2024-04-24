<template>
   <div class="el-select el-select--multiple">
      <div class="el-input el-input--suffix">
         <div @click.stop="showDropdown" class="el-input__inner">
            <div>
               <input v-if="!selectedItems?.length" :class="{ 'is-focus': isDropdownVisible }" v-model="searchText"
                  @input="filterOptions" @focus="showDropdown" />
               <div v-else class="el-input__value">
                  <span v-for="(item, index) in selectedItems" :key="index" class="el-tag el-tag--info el-tag--mini"
                     @click.stop="removeItem(item)">
                     {{ item }} <img src="@/assets/img/close.svg" />
                  </span>
               </div>
               <i class="el-input__suffix el-icon-arrow-up" :class="{ 'is-reverse': isDropdownVisible }"
                  @click.stop="toggleDropdown">
                  <img src="@/assets/img/arrow.svg" />
               </i>
            </div>
         </div>
      </div>
      <transition name="el-zoom-in-top">
         <div v-if="isDropdownVisible" class="el-select-dropdown el-popper">
            <div class="el-scrollbar">
               <ul class="el-select-dropdown__list">
                  <li v-for="(option, index) in filteredOptions" :key="index" class="el-select-dropdown__item"
                     :class="selectedItems.includes(option) && 'el-select-dropdown__item--active'"
                     @click="toggleSelection(option)">
                     <span style="font-size: 14px; margin-right: 50px;">{{ option }} </span>
                     <img v-if="selectedItems.includes(option)" src="@/assets/img/check.svg" alt="">
                  </li>
               </ul>
            </div>
         </div>
      </transition>
   </div>
</template>

<script setup>
import { ref, computed } from 'vue';


const props = defineProps({
   options: {
      type: Array,
      required: true
   },
   modelValue: {
      type: Array,
      default: []
   }
})

const selectedItems = ref(props.modelValue);
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


const toggleDropdown = () => {
   isDropdownVisible.value = !isDropdownVisible.value;
};
 

</script>


<style scoped lang="scss">
/* Styles to mimic Element Plus multiselect */
.el-select {
   width: 100%;
}

.el-input__value {
   border: 1px solid black;
   border-radius: 6px;
   min-height: 48px;
   padding: 5px 16px;
   padding-right: 60px;
}

.el-input {
   display: flex;
   align-items: center;
   justify-content: space-between;
}

.el-input__inner {
   position: relative;
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

.el-icon-arrow-up {
   position: absolute;
   top: 11px;
   right: 6px;
   z-index: 999;

   img {
      width: 24px;
      height: 24px;
      cursor: pointer;
   }
}

.el-icon-arrow-up.is-reverse {
   transform: rotate(90deg);

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
   min-width: 200px;
}

.el-select-dropdown__item {
   padding: 4px 8px;
   cursor: pointer;
   display: flex;
   justify-content: space-between;
}

.el-select-dropdown__item:hover {
   background-color: #f5f7fa;
}

.el-popper {
   margin-top: 4px;
}

.el-select-dropdown__item--active {
   background: #f3f1f1 !important;
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