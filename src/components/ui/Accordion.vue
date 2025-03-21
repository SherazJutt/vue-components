<script setup lang="ts">
import { ref } from "vue";

// Props definition
// `items` is an array of objects where each object represents an accordion item.
// Each item has a `title` (string), `content` (string), and an optional `slot` (string) for custom slot rendering.
defineProps({
	items: { type: Array as () => { title: string; content: string; slot?: string }[], required: true },
});

// This ref holds the index of the currently active (opened) accordion item.
// If `null`, no accordion item is open.
const activeIndex = ref<Number | null>(null);
</script>

<template>
	<!-- Accordion Container -->
	<div class="w-full divide-y divide-gray-200 rounded border border-slate-200 bg-white">
		<!-- Iterate through items array -->
		<div v-for="(item, index) in items" :key="index" v-if="items.length">
			<!-- Accordion Header -->
			<!-- Clicking toggles activeIndex between `index` and `null` (for collapse) -->
			<div :class="{ 'bg-primary/5': activeIndex === index }" class="hover:bg-primary/5 relative flex cursor-pointer justify-between gap-4 p-3 font-medium duration-100" @click="activeIndex = activeIndex === index ? null : index">
				<!-- Accordion Title -->
				<span class="title text-gray-600">{{ item.title }}</span>
				<!-- Chevron Icon -->
				<Icon icon="jam:chevron-down" :class="{ 'rotate-180': activeIndex === index }" class="icon text-[1.4rem] text-gray-400 duration-200" />
			</div>

			<!-- Accordion Body -->
			<div v-if="activeIndex === index" class="p-3 text-gray-500">
				<!-- If slot name is provided, render slot content -->
				<template v-if="item.slot">
					<slot :name="item.slot" />
				</template>
				<!-- Otherwise, render static content -->
				<template v-else>
					{{ item.content }}
				</template>
			</div>
		</div>

		<!-- Fallback message if no items provided -->
		<div class="px-3 py-6 text-center text-gray-500" v-else>No items to show</div>
	</div>
</template>
