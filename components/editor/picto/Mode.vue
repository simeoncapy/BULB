<script setup lang="ts">
const {
  mode,
} = defineProps<{
  mode: Mode | null
}>()

const circle = computed(() => mode === 'METRO')
const roundRectangle = computed(() => ['RER', 'TER', 'TRAIN', 'TRAIN_RER'].includes(mode ?? ''))
const square = computed(() => ['BOAT', 'BUS', 'BRT', 'CABLE', 'TRAM', 'VELO'].includes(mode ?? ''))
</script>

<template>
  <div class="relative picto-wrapper">
    <div class="absolute" :class="{ circle, 'round-rectangle': roundRectangle, square }" />
    <div
      class="absolute picto" :class="{
        boat: mode === 'BOAT',
        brt: mode === 'BRT',
        bus: mode === 'BUS',
        cable: mode === 'CABLE',
        metro: mode === 'METRO',
        rer: mode === 'RER',
        train: mode === 'TRAIN',
        train_rer: mode === 'TRAIN_RER',
        tram: mode === 'TRAM',
        velo: mode === 'VELO',
        empty: mode === null,
      }"
    />
  </div>
</template>

<style scoped lang="scss">
.picto-wrapper {
  min-width: 1em;
  min-height: 1em;
}

.picto {
  min-width: 1em;
  min-height: 1em;
  background-color: var(--blue-ratp-paper);

  &.empty {
    background: transparent;
  }
}

.circle {
  border-radius: 50%;
  background: white;
  top: .0625em;
  left: .0625em;
  min-width: .875em;
  min-height: .875em;
}

.round-rectangle {
  border-radius: .125em;
  background: white;
  top: .0625em;
  left: .0625em;
  min-width: .875em;
  min-height: .875em;
}

.square {
  border-radius: .01em;
  background: white;
  top: .025em;
  min-width: 1em;
  min-height: .95em;
}

.boat {
  mask: url(assets/svg/modes/boat.svg) no-repeat center;
}

.brt {
  mask: url(assets/svg/modes/brt.svg) no-repeat center;
  mask-size: contain;
}

.bus {
  mask: url(assets/svg/modes/bus.svg) no-repeat center;
}

.cable {
  mask: url(assets/svg/modes/cable.svg) no-repeat center;
}

.metro {
  mask: url(assets/svg/modes/metro.svg) no-repeat center;
}

.rer {
  mask: url(assets/svg/modes/rer.svg) no-repeat center;
}

.ter {
  mask: url(assets/svg/modes/ter.svg) no-repeat center;
}

.train {
  mask: url(assets/svg/modes/train.svg) no-repeat center;
}

.train_rer {
  mask: url(assets/svg/modes/train_rer.svg) no-repeat center;
}

.tram {
  mask: url(assets/svg/modes/tram.svg) no-repeat center;
}

.velo {
  mask: url(assets/svg/modes/velo.svg) no-repeat center;
}
</style>
