<template>
  <div :style="{ color: displayStyle.fontColor.value, fontSize: displayStyle.fontSize.value }">
    <strong>App state: "{{ data.state.value }}"</strong>
    <br>
    <button @click="cycleState">Cycle State</button>
  </div>
</template>

<script lang="ts">
import { computed, defineComponent, PropType, reactive, Ref, toRefs, ToRefs } from 'vue';

type FontSize = number | string;
type FontColor = 'blue' | 'red' | 'green';

interface AppProps {
  fontSize: FontSize;
  fontColor: FontColor;
}

interface AppData {
  state: 'state1' | 'state2' | 'state3';
}

export default defineComponent({
  props: {
    fontSize: [Number, String] as PropType<FontSize>,
    fontColor: String as PropType<FontColor>,
  },
  setup(props: Partial<AppProps>) {
    const data: ToRefs<AppData> = toRefs(reactive({
      // state property becomes `Ref<'state1' | 'state2' | 'state3'>` in 3.0.2,
      // but it becomes `Ref<'state1'> | Ref<'state2'> | Ref<'state3'>`
      state: 'state1',
    }));

    const cycleState = () => {
      switch (data.state.value) {
        case 'state1':
          data.state.value = 'state2';
          break;
        case 'state2':
          data.state.value = 'state3';
          break;
        case 'state3':
          data.state.value = 'state1';
          break;
      }
    };

    const displayStyle: ToRefs<AppProps> = {
      // fontSize property has de-aliased type `Ref<number | string>` in 3.0.2,
      // which accepts an assignment of a `ComputedRef<FontSize>` value since
      // `type FontSize = number | string` and ComputedRef extends Ref
      // in 3.0.3 fontSize has incompatible type `Ref<number> | Ref<string>`
      fontSize: computed<FontSize>(() => props.fontSize ?? '24px'),
      fontColor: computed<FontColor>(() => props.fontColor ?? 'blue')
    };

    return {
      data,
      displayStyle,
      cycleState,
    };
  },
});
</script>
