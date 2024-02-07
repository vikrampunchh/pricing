<template>
  <span :class="containerClass">
    {{ prefixLabel }}
    <span v-if="!isEditInput" :class="labelClass" @dblclick="editInput('base_price', index)">{{ valueType }}{{ value }} </span>
    <input
      v-show="isEditInput"
      ref="input"
      class="label-input"
      v-on:blur="hideInput(), $emit('blur', $event)"
      @keyup.enter="hideInput()"
      v-model="editInputValue"
    />
    {{ suffixLabel }}
  </span>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'EditableLabel',
  props: {
    prefixLabel: String,
    suffixLabel: String,
    labelClass: String,
    containerClass: String,
    value: String,
    valueType: String,
    updateKey: String,
    index: Number,
    plan: Object,
    onValueChange: Function,
  },
  setup(props) {
    const input = ref(null)
    const isEditInput = ref('')
    const editInputValue = ref(props.plan[props.updateKey])

    const parseNumber = function (num) {
      if (Number.isNaN(Number(num))) {
        return 0
      }
      return Number(num)
    }

    const editInput = function () {
      isEditInput.value = true
      console.log('editInput', input.value.focus)
      setTimeout(() => {
        input.value.focus()
      }, 100)
    }

    const hideInput = function () {
      console.log('hideInput', editInputValue.value, isEditInput.value, parseNumber(editInputValue.value))
      if (!editInputValue.value || !isEditInput.value || !parseNumber(editInputValue.value)) {
        isEditInput.value = false
        return
      }

      const newPlan = Object.assign(props.plan, {})
      newPlan[props.updateKey] = parseNumber(editInputValue.value)
      props.onValueChange(newPlan, props.index)

      isEditInput.value = false
    }

    return {
      input,
      isEditInput,
      editInputValue,
      editInput,
      hideInput,
    }
  },
}
</script>
