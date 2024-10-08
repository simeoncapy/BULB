<script setup lang="ts">
import { VueDraggable } from 'vue-draggable-plus'

const {
  inner = false,
  fluid = false,
} = defineProps<{
  inner?: boolean
  fluid?: boolean
}>()
const section = defineModel<LineSection>({ required: true })
const elements = computed({
  get: () => section.value.$lineSection.elements,
  set: val => section.value.$lineSection.elements = val,
})

const offset = computed(() => `calc(${section.value.$lineSection.levelOffset} * -2.75em)`)
</script>

<template>
  <div
    class="section" :class="{
      inner,
      fluid,
      empty: elements.length === 0,
    }"
  >
    <VueDraggable
      v-model="elements"
      :animation="150"
      class="elements open"
      group="sectionElements"
      ghost-class="section-ghost"
      :swap-threshold="inner ? .5 : .25"
    >
      <SectionElement
        v-for="(element, i) in elements"
        :key="element.id"
        v-model="elements[i]"
        :fluid="fluid"
      />
    </VueDraggable>
  </div>
</template>

<style>
.section-ghost {
  opacity: .5;
}
</style>

<style scoped lang="scss">
.section {
  position: relative;
  display: flex;
  flex-direction: column;
  flex-shrink: 0;
  gap: .5em;
  transform: translateY(v-bind(offset));
  min-width: 1em;
  outline: 1px dashed transparent;
  border-radius: .25em;
  transition: outline-color .3s ease;

  &.fluid {
    width: 100%;
  }

  .section:hover > &, &:hover, &.empty {
    outline-color: var(--p-slate-300);
  }
  &.inner.empty {
    outline-color: var(--p-orange-300);
  }

  .section-handle {
    position: absolute;
    top: .125em;
    left: .125em;
    cursor: grab;

    i {
      display: block;
      color: var(--p-slate-400);
      font-size: .375em;
    }
  }
}

.elements {
  display: flex;
  flex-direction: row;
  align-items: stretch;

  .inner > & {
    min-height: 2em;
  }
  :not(.inner) > & {
    min-height: 5em;
  }

  :not(.inner) > & > :not(.placeholder) {
    &:first-child {
      padding-left: 1em;
    }
    &:last-child {
      padding-right: 1em;
    }
  }

  .placeholder {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }
}

.open {
  padding: 0 2em;
  margin: 0 -2em;

  .debug & {
    outline: 1px dashed lime;
  }

  .elements .element:not(:last-child) & {
    padding-right: 0;
    margin-right: 0;
  }

  .elements .element:not(:first-child) & {
    padding-left: 0;
    margin-left: 0;
  }
}
</style>
