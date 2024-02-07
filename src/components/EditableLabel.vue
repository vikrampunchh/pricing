<template>
  <span :class="containerClass">
    {{ prefixLabel }}
    <span v-if="!isEditInput" :class="labelClass" @dblclick="editInput('base_price', index)">{{ valueType }}{{ value }} </span>
    <input v-if="isEditInput" class="label-input" v-on:blur="hideInput()" @keyup.enter="hideInput()" v-model="editInputValue" v-focus="true" />
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
    }

    const hideInput = function (label, index) {
      if (!editInputValue.value || !isEditInput.value || !parseNumber(editInputValue.value)) {
        isEditInput.value = false
        return
      }

      const newPlan = Object.assign(props.plan, {})
      newPlan[props.updateKey] = parseNumber(editInputValue.value)
      props.onValueChange(newPlan, index)

      isEditInput.value = false
      editInputValue.value = ''
    }

    return {
      isEditInput,
      editInputValue,
      editInput,
      hideInput,
    }
  },
}
</script>
