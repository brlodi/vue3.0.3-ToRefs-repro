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
