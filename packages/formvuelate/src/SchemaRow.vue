<template>
  <div
    v-if="rowHasVisibleElements"
    :class="['schema-row', schemaRowClasses]"
  >
    <SchemaField
      v-for="field in row"
      :key="field.model"
      :field="field"
      v-bind="$attrs"
      class="schema-col"
    />
  </div>
</template>

<script>
import SchemaField from './SchemaField.vue'

import { computed, inject } from 'vue'
import { FORM_MODEL } from './utils/constants'

export default {
  name: 'SchemaRow',
  components: { SchemaField },

  props: {
    row: {
      type: Array,
      required: true
    },
    schemaRowClasses: {
      type: [String, Object, Array],
      default: null
    }
  },

  setup (props) {
    const formModel = inject(FORM_MODEL, {})

    const rowHasVisibleElements = computed(() => {
      for (const field of props.row) {
        // If a field doesnt have a condition it guarantees itll be rendered
        if (!field.condition) return true

        // If a field condition is true, it will be rendered
        if (typeof condition !== 'function' && field.condition(formModel.value) === true) return true
      }

      return false
    })

    return {
      rowHasVisibleElements
    }
  }
}
</script>
