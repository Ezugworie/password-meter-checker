<template>
  <div class="flex flex-col">
    <h1 class="mb-1 font-medium md:text-xl px-2">Password Strength Checker</h1>
    <app-input 
      label="password" 
      class=""
      v-model="password"
      ref="inputFocus"
      type="text"
    />
    <password-meter 
      min="0" 
      max="100" 
      low="21" 
      high="68" 
      optimum="70" 
      :value="strength"
      class="md:px-3"
    />

    <div class="flex flex-col justify-start text-left">
      <h1 class="text-gray-600">Rules</h1>
      <ul class="text-sm">
        <li :class="assertClass.length">password must have 6 or more characters </li>
        <li :class="assertClass.lowercase">must contain a lowercase letter</li>
        <li :class="assertClass.uppercase">must contain an <span class="uppercase">uppercase</span> letter</li>
        <li :class="assertClass.number">must contain a number</li>
        <li :class="assertClass.specialCharacter">must contain a special character like />'";,^*()-_</li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { defineProps, reactive, ref, watchEffect, watch, onMounted, nextTick } from 'vue'
import AppInput from './AppInput.vue'
import PasswordMeter from './PasswordMeter.vue'

defineProps({
  msg: String
})

const conditions = reactive({
  passwordLength: false,
  uppercase: false,
  lowercase: false,
  number: false,
  specialCharacter: false,
})
const password = ref('')
const strength = ref(0)
const strengthSummary = ref(new Set())
const matcher = reactive({
  uppercase: RegExp('([A-Z])+'),
  lowercase: RegExp('([a-z])+'),
  number: RegExp('([0-9])+'),
  specialCharacter: RegExp('[^a-zA-Z0-9]+')
})
const assertClass = reactive({
  length: '',
  uppercase: '',
  lowercase: '',
  number: '',
  specialCharacter: ''
})
const inputFocus = ref()

const addStrength = ((args) => {
  strengthSummary.value.add(args)
  assertClass[args] = 'text-green-500'
})

const deleteStrength = ((args) => {
  strengthSummary.value.delete(args)
    assertClass[args] = 'text-red-500'

})

  watch(password, (password, prevPassword) => {
        conditions.passwordLength = password.length > 5 ? true : false
        conditions.lowercase = 
          (password.match(matcher.lowercase) !== null) ? true : false
        conditions.uppercase = 
          (password.match(matcher.uppercase) !== null) ? true : false
        conditions.number = 
          (password.match(matcher.number) !== null) ? true : false
        conditions.specialCharacter = 
          (password.match(matcher.specialCharacter) !== null) ? true : false


        conditions.passwordLength ? addStrength('length') : deleteStrength('length')
        conditions.lowercase ? addStrength('lowercase') : deleteStrength('lowercase')
        conditions.uppercase ? addStrength('uppercase') : deleteStrength('uppercase')
        conditions.number ? addStrength('number') : deleteStrength('number')
        conditions.specialCharacter ? addStrength('specialCharacter') : deleteStrength('specialCharacter')
        
        strength.value = strengthSummary.value.size * 20

  })

  onMounted(() => {
    //TODO focus on input onMount
    nextTick(() => {
      // input.focus()
    })
  });
</script>