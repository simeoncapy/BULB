<script setup lang="ts">
import { isMode, isService } from '~/utils/types'

const {
  index,
} = defineProps<{
  index: number
}>()

const emit = defineEmits<{
  delete: [number]
}>()
const connection = defineModel<Connection>('connection', { required: true })
</script>

<template>
  <Panel
    class="md:min-w-35em md:max-w-40em"
    :header="$t('ui.dialogs.connections_editor.group.header', { index: index + 1 })"
    :pt="{
      root: { class: 'flex flex-col' },
      content: { class: 'h-full' },
      contentContainer: { class: 'h-full' },
    }"
  >
    <template #icons>
      <Tag v-if="isMode(connection)" severity="warn">
        {{ $t('ui.dialogs.connections_editor.mode') }}
      </Tag>
      <Tag v-if="isService(connection)" severity="info">
        {{ $t('ui.dialogs.connections_editor.service') }}
      </Tag>
    </template>

    <div class="h-full flex flex-col gap-2 justify-between">
      <ModeConnectionGroupEditor
        v-if="isMode(connection)"
        v-model:connection="connection"
      />
      <ServiceConnectionGroupEditor
        v-else-if="isService(connection)"
        v-model:connection="connection"
      />
      <div v-else />

      <div class="flex flex-row items-center justify-between gap-2">
        <div />
        <Button :label="$t('ui.dialogs.connections_editor.group.delete')" size="small" icon="i-tabler-trash" severity="danger" @click="emit('delete', index)" />
      </div>
    </div>
  </Panel>
</template>
