<script setup lang="ts">
import type { TresObject } from '@tresjs/core'
import { useLoop } from '@tresjs/core'
import { shallowRef } from 'vue'
import { BloomPmndrs, EffectComposerPmndrs } from '@tresjs/post-processing';
import ColorSwitchPuzzle from './ColorSwitchPuzzle.vue';
import type { PuzzleState } from './ColorSwitchPuzzle.vue';

const props = defineProps({
  puzzleInput: {
    type: String,
    required: true,
  },
  wireframesInput: {
    type: String,
    required: true,
  },
  colorOn: {
    type: String,
    required: true,
  },
  colorOff: {
    type: String,
    required: true
  }
});

const emit = defineEmits<{
  puzzleStateChange: [state: PuzzleState]
}>()

const { onBeforeRender } = useLoop()

const boxRef = shallowRef<TresObject | null>(null)

onBeforeRender(({ elapsed }) => {
  if (boxRef.value) {
    boxRef.value.rotation.y = elapsed
    boxRef.value.rotation.z = elapsed
  }
})
</script>

<template>
  <TresPerspectiveCamera :position="[0, 15, 4]" :look-at="[0, 0, 0]" />
  <ColorSwitchPuzzle
    :puzzleSetup="puzzleInput"
    :wireframe-setup="wireframesInput"
    :colorOn="$props.colorOn"
    :colorOff="$props.colorOff"
    @puzzle-state-change="emit('puzzleStateChange', $event)"
  />
  <TresAxesHelper />
  <TresGridHelper :args="[10, 10]" />
  <Suspense>
  <EffectComposerPmndrs>
    <BloomPmndrs
      :intensity="1.6"
      :luminance-threshold="0.2"
      :luminance-smoothing="0.9"
      :radius="0.4"
    />
  </EffectComposerPmndrs>
  </Suspense>
</template>
