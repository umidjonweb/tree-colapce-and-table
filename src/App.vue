<template>
   <div class="list">
      <div style="max-width: fit-content; width: 100%; padding: 10px; ">
         <TreeCollapse v-for="item in lists" :key="item.id" :depth="0" :item="item" />
      </div>
      <div>
         <DynamicTable @edit="editTable" @delete="deleteTable" :data="tableData" :columns="tableColumns">
         </DynamicTable>
         <Modal ref="openModal" @close="closeModal">
            <h2>Modal Title</h2>
            <form action="form">
               <div class="form-item">
                  <div>
                     <label style="display: block; margin-bottom: 10px;;" for="age">Age</label>
                     <input v-model="_formData.age" type="text">
                  </div>
                  <div>
                     <label style="display: block; margin-bottom: 10px;;" for="name">Name</label>
                     <input v-model="_formData.name" type="text">
                  </div>
                  <div>
                     <label style="display: block; margin-bottom: 10px;;" for="address">Address</label>
                     <input v-model="_formData.address" type="text">
                  </div>
                  <div>
                     <label style="display: block; margin-bottom: 10px;;" for="css">CSS</label>
                     <select v-model="_formData.tags" name="test[]" size="3" multiple>
                        <option aria-rowspan="5" v-for="option in options" :key="option" :value="option">{{ option }}
                        </option>
                     </select>
                  </div>
               </div>
               <div class="form-btn">
                  <button type="button" @click="openModal.open(false)" class="cancel">Cancel</button>
                  <button type="button" @click="submit" class="submit">Submit</button>
               </div>
            </form>
         </Modal>
      </div>
   </div>
</template>

<script setup>
import { ref } from "vue";
import TreeCollapse from "@/components/TreeCollapse.vue";
import MultiSelect from "@/components/MultiSelect.vue";
import Modal from "@/components/Modal.vue";
import DynamicTable from "@/components/dinamicTable.vue";
import { lists } from '@/data'

const tableData = ref([
   { id: 1, name: 'John', address: 'Buxoro', tags: ["NICE", "DEVELOPER", "LOSER"], age: 30 },
   { id: 2, name: 'Jane', address: 'Toshkent', tags: ["LOSER"], age: 25 },
   { id: 3, name: 'Doe', address: 'Navoiy', tags: ["DEVELOPER", "LOSER"], age: 40 }
])

const tableColumns = [
   { key: 'id', label: 'ID' },
   { key: 'name', label: 'Name' },
   { key: 'age', label: 'Age' },
   { key: 'address', label: 'Address' },
   { key: 'tags', label: 'Tags', editable: true },
]

const options = ["NICE", "DEVELOPER", "LOSER"]

const openModal = ref()

const _formData = ref({
   id: 0,
   name: '',
   address: '',
   tags: [],
   age: 0,
})
function deleteTable(ind) {
   tableData.value.splice(ind, 1)
}
function editTable(item) {
   console.log(item);
   _formData.value = JSON.parse(JSON.stringify(item))
   openModal.value.open(item)
}

function submit() {
   tableData.value = tableData.value.map((item => item.id == _formData.value.id ? _formData.value : item))
   openModal.value.open(false)
}

</script>

<style scoped>
.list {
   padding: 40px;
}

.form-item {
   padding: 10px;
   display: grid;
   gap: 20px;
   grid-template-columns: 50% 50%;
}

.form-btn {
   display: flex;
   justify-content: end;
   align-items: center;
   padding: 20px;
   gap: 20px;
}
</style>
