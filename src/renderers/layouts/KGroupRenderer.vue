<template>
  <KCard
    v-if="layout.visible"
    :title="layout.label"
  >
    <div
      v-for="(element, index) in layout.uischema.elements"
      :key="`${layout.path}-${index}`"
      :class="styles.group.item"
    >
      <dispatch-renderer
        :schema="layout.schema"
        :uischema="element"
        :path="layout.path"
        :enabled="layout.enabled"
        :renderers="layout.renderers"
        :cells="layout.cells"
      />
    </div>
  </KCard>
</template>

<script lang="ts">
import {
  // JsonFormsRendererRegistryEntry,
  Layout,
  rankWith,
  and,
  isLayout,
  uiTypeIs,
} from '@jsonforms/core';
import { defineComponent } from 'vue';
import {
  DispatchRenderer,
  rendererProps,
  useJsonFormsLayout,
  RendererProps,
} from '@jsonforms/vue';
import { useVanillaLayout } from '../../util';

const layoutRenderer = defineComponent({
  name: 'GroupRenderer',
  components: {
    DispatchRenderer,
  },
  props: {
    ...rendererProps<Layout>(),
  },
  setup(props: RendererProps<Layout>) {
    return useVanillaLayout(useJsonFormsLayout(props));
  },
});

export default layoutRenderer;

// export const entry: JsonFormsRendererRegistryEntry = {
//   renderer: layoutRenderer,
//   tester: rankWith(2, and(isLayout, uiTypeIs('Group'))),
// };

export const tester = rankWith(2, and(isLayout, uiTypeIs('Group')))
</script>
