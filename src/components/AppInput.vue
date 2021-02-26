<template>
  <Form v-slot="{ errors }" class="flex flex-col w-full md:w-1/3">
    <div class="flex border border-gray-300 rounded-md focus:outline-none py-2 m-2 text-gray-600 font-medium justify-center space-x-1">
      <input 
        name="field" 
        :rules="isRequired" 
        :type="type"
        :value="modelValue"
        :placeholder="label"
        @input="$emit('update:modelValue', $event.target.value)"
        @change="$emit('update:modelValue', $event.target.value)"
        class="focus:outline-none md:w-full px-1"
    />
      <!-- <span 
        class="cursor-pointer"
        :class="visibleProp.class"
        @click="toggleVisibility">
          eye
      </span> -->
    </div>
    <span class="text-sm text-red-500">{{ errors.field }}</span>
  </Form>
</template>

<script lang="ts">
  import { defineComponent, onUpdated, PropType, reactive, ref, watchEffect } from 'vue'
  import { Field, Form } from 'vee-validate'
 
type InputType = 'text'|'tel'|'email'|'number'|'password'

export default defineComponent({
  components: {
    Field,
    Form
  },
  props: {
    label: {
      required: true,
      type:String,
      default: 'Password'
    },
    type: {
      required: true,
      type: String as PropType<'password'|'text'>,
    },
    modelValue: {
      required: true,
      type: String,
    }
  },
  setup(props, emit) {
    const isRequired = ((value) => {
      return value ? true : 'This field is required';
    });

    // const visibleProp = reactive<String>({
    //   class: 'line-through',
    // })
    // const toggleVisibility = (() => {
    //   visibleProp.class = (visibleProp.class === null ? 'line-through' : null)
    //   props.type = (props.type === 'password' ? 'text' : 'password')
    // })

    return {
      isRequired,
      // visibleProp,
      // toggleVisibility,
    }
  }
});
</script>
<style>

</style>