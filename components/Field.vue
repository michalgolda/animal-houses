<template>
    <div class="flex flex-col" :class="{ 'gap-y-1': label || error }">
        <div class="flex flex-col">
            <label :for="`${name}-input`" v-if="label" :class="{ 'text-red-500': error }"
                class="font-semibold text-sm text-secondary">{{ label
                }}</label>
            <p class="text-red-500 text-sm" v-if="error">{{ error }}</p>
        </div>
        <component v-if="is === 'input'" :is="is" :id="id"
            :class="{ 'border': error, 'border-red-500': error, 'border-solid': error, 'focus:border-red-500': error }"
            class="focus:border focus:border-secondary focus:border-solid rounded bg-gray-50 p-2 text-tertiary text-sm outline-none text-color-slate-700 border border-transparent"
            v-bind="$attrs" />
        <component v-if="is === 'select'" :is="is" :id="id"
            :class="{ 'border': error, 'border-red-500': error, 'border-solid': error, 'focus:border-red-500': error }"
            class="focus:border focus:border-secondary focus:border-solid rounded text-tertiary bg-gray-50 p-2 text-sm outline-none text-color-slate-700 border border-transparent"
            v-bind="$attrs">
            <slot></slot>
        </component>
    </div>
</template>

<script setup lang="ts">
export interface Props {
    name: string
    label?: string
    error?: string
    is?: "input" | "select"
}

const props = withDefaults(defineProps<Props>(), {
    is: "input"
})

const id = computed(() => `${props.name}-${props.is}`)
</script>