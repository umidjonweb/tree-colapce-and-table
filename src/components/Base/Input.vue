<script lang="ts" setup>
import { computed } from 'vue';
const props = defineProps({
    label: {
        type: String,
        default: ''
    },
    id: {
        type: String,
        default: Math.random().toString()
    },
    placeholder: {
        type: String,
        default: ''
    },
    textarea: {
        type: Boolean,
        default: false
    },
    modelValue: {
        type: String || Number || Object,
        default: ''
    },
    required: {
        type: Boolean,
        default: false
   },
   type: {
      type: String,
      default: 'text'
   }
})
const emit = defineEmits(['update:modelValue', 'change'])

const value = computed({
    get() {
        return props.modelValue
    },
    set(value) {
        emit('change', value)
        emit('update:modelValue', value)
    }
})

</script>

<template>
    <div class="flex-col">
        <label :for="props.id" >{{ props.label }} <span v-if="props.required" class="text-danger">*</span> </label>
        <input :required="props.required" v-model="value" v-if="!props.textarea" :placeholder="placeholder" :type="props.type" :id="props.id" class="mt-1">
        <textarea :required="props.required" v-model="value" v-else cols="4" rows="5" :placeholder="placeholder" :id="props.id" class="mt-1"></textarea>
    </div>
</template>

<style lang="scss">
.flex-col{
   display: flex;
   flex-direction: column
}
.text-danger{
   background: red;
}
.mt-1{
   margin-top: 4px;
}

</style>