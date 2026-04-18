<script setup lang="ts">
// No imports needed! TresJS components are available globally
import { BoxGeometry } from 'three'

/** Shared source geometry for `EdgesGeometry` (lines require `LineBasicMaterial`, not mesh materials). */
const outlineSourceGeometry = new BoxGeometry(1.05, 1.05, 1.05)

  const props = defineProps({
    pos_x: {
      type: Number,
      required: true,
    },
    pos_y: {
      type: Number,
      required: true,
    },
    pos_z: {
      type: Number,
      required: true,
    },
    colorOn: {
      type: String,
      required: true,
    },
    colorOff: {
      type: String,
      required: true,
    },
    isOn: {
        type: Boolean,
        default: false,
    },
    showX: {
      type: Boolean,
      default: true,
    }
  });

  const emit = defineEmits(['toggle']);

//   const geometry = new THREE.BoxGeometry();
// const edges = new THREE.EdgesGeometry( geometry );
// const line = new THREE.LineSegments( edges );
// scene.add( line );

</script>

<template>
  <TresGroup :position="[props.pos_x, props.pos_y, props.pos_z]">
    <TresMesh @click="emit('toggle')">
      <TresBoxGeometry :args="[1, 1, 1]" />
      <TresMeshStandardMaterial
        :color="props.isOn ? props.colorOn : props.colorOff"
        :emissive="props.isOn ? props.colorOn : props.colorOff"
        :emissiveIntensity="1"
        :metalness="0.2"
        :roughness="0.8"
      />
    </TresMesh>
    <TresLineSegments v-if="props.showX">
      <TresEdgesGeometry :args="[outlineSourceGeometry]" />
      <TresLineBasicMaterial color="#ffffff" />
    </TresLineSegments>
  </TresGroup>
</template>
