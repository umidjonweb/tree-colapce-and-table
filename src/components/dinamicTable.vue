<script lang="ts" setup>
import { ref } from 'vue';

const props = defineProps({
   columns: {
      type: Array,
      required: true,
   },
   data: {
      type: Array,
      required: true,
   }
})
const emit = defineEmits(['edit', 'delete'])
const highlightedColumn = ref(null)

function highlightColumn(index) {
   this.highlightedColumn = index;
}
function resetColumnHighlight() {
   this.highlightedColumn = null;
}

function editRow(rowIndex) {
   emit('edit', rowIndex);
}

function deleteRow(rowIndex) {
   emit('delete', rowIndex);
}
</script>

<template>
   <div>
      <table class="custom-table">
         <thead>
            <tr>
               <th v-for="(column, index) in columns" :key="index" @mouseenter="highlightColumn(index)"
                  @mouseleave="resetColumnHighlight()">
                  {{ column.label }}
               </th>
               <th>Actions</th>
            </tr>
         </thead>
         <tbody>
            <tr v-for="(item, rowIndex) in data" :key="rowIndex">
               <td v-for="(column, colIndex) in columns" :key="colIndex"
                  :class="{ 'highlighted': highlightedColumn === colIndex }">
                  <template v-if="column.editable">
                     <div class="col-flex">
                        <div v-for="tagColumn, ind in item.tags" :key="'ind' + ind">
                         <button :class="{
                           'secondary1': tagColumn == 'DEVELOPER',
                           'green': tagColumn == 'NICE',
                           'primary': tagColumn == 'LOSER',

                        }">{{ tagColumn }}</button>
                        </div>
                     </div>
                  </template>
                  <template v-else>
                     {{ item[column.key] }}
                  </template>
               </td>
               <td>
                  <button class="secondary" @click="editRow(item)">Edit</button>
                  <button class="cancel" @click="deleteRow(rowIndex)">Delete</button>
               </td>
            </tr>
         </tbody>
      </table>
   </div>

</template>
<style lang="scss" scoped>
.custom-table {
   border-collapse: collapse;
   width: 100%;
}
.secondary1{
   color: rgb(243, 132, 36);
   border: 2px solid rgb(243, 132, 36);
   background: white;
}
.green{
   color: rgb(36, 243, 70);
   border: 2px solid rgb(36, 243, 70);
   background: white;
}
.primary{
   color: rgb(36, 102, 243);
   border: 2px solid rgb(36, 102, 243);
   background: white;
}

.col-flex {
   display: flex;
   gap: 20px;
   flex-wrap: wrap;
}

button {
   cursor: pointer;
}

.custom-table th,
.custom-table td {
   border: 1px solid #dddddd;
   padding: 8px;
   text-align: left;
}

.custom-table th {
   background-color: #f2f2f2;
}

.custom-table tr:hover {
   background-color: #f5f5f5;
}

.custom-table .highlighted {
   background-color: #e6f7ff;
   /* Change this color as per your preference */
}
</style>