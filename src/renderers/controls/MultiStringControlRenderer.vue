<template>
  <control-wrapper
    v-bind="controlWrapper"
    :styles="styles"
    :is-focused="isFocused"
    :applied-options="appliedOptions"
  >
    <textarea
      :id="control.id + '-input'"
      :class="styles.control.textarea"
      :value="control.data"
      :disabled="!control.enabled"
      :autofocus="appliedOptions.focus"
      :placeholder="appliedOptions.placeholder"
      @change="onChange"
      @focus="isFocused = true"
      @blur="isFocused = false"
    />
  </control-wrapper>
</template>

<script lang="ts">
import {
  ControlElement,
  // JsonFormsRendererRegistryEntry,
  rankWith,
  isStringControl,
  isMultiLineControl,
  and,
  RankedTester,
} from '@jsonforms/core';
import { defineComponent } from 'vue';
import {
  rendererProps,
  useJsonFormsControl,
  RendererProps,
} from '@jsonforms/vue';
import ControlWrapper from './ControlWrapper.vue';
import { useVanillaControl } from '../../util';

const controlRenderer = defineComponent({
  name: 'MultiStringControlRenderer',
  components: {
    ControlWrapper,
  },
  props: {
    ...rendererProps<ControlElement>(),
  },
  setup(props: RendererProps<ControlElement>) {
    return useVanillaControl(
      useJsonFormsControl(props),
      (target) => target.value || undefined
    );
  },
});

export default controlRenderer;

// export const entry: JsonFormsRendererRegistryEntry = {
//   renderer: controlRenderer,
//   tester: rankWith(2, and(isStringControl, isMultiLineControl)),
// };

export const tester: RankedTester = rankWith(2, and(isStringControl, isMultiLineControl));
</script>
