<script setup lang="ts">
import { computed, ref } from "vue";
import Button from "./Button.vue";

const props = defineProps({
	modelValue: { type: String, default: "" }, // v-model binding
	options: { type: Array as () => string[], required: true }, // Select options
	variant: { type: String as () => "outlined" | "text" | "ghost" | "solid", default: "solid" },
});

const emit = defineEmits(["update:modelValue"]);

const isOpen = ref(true);
const selectedOption = ref(props.modelValue);

const selectOption = (option: string) => {
	selectedOption.value = option;
	emit("update:modelValue", option);
	isOpen.value = false;
};

const searchQuery = ref("");

// Computed for filtering options based on search input
const filteredOptions = computed(() => {
	if (!searchQuery.value) return props.options;
	return props.options.filter((option) => option.toLowerCase().includes(searchQuery.value.toLowerCase()));
});
</script>

<template>
	<div class="relative">
		<!-- select trigger -->
		<Button :icon-class="isOpen ? 'rotate-180' : ''" icon="ci:chevron-down" class="w-full" :variant="variant" :label="selectedOption || 'Select'" @click="isOpen = !isOpen" />

		<!-- select menu -->
		<ul v-if="isOpen" class="absolute top-full z-10 mt-1 flex w-full list-none flex-col rounded border border-gray-200 bg-white shadow-md shadow-slate-500/10">
			<li class="border-b border-gray-200 p-2">
				<input type="text" v-model="searchQuery" class="focus:border-primary w-full rounded-sm border border-gray-200 px-2 py-1 focus:outline-none" placeholder="Search" />
			</li>
			<li v-for="(option, index) in filteredOptions" :key="index" :class="option === selectedOption && 'bg-primary/10'" class="hover:bg-primary/5 cursor-pointer px-4 py-2" @click="selectOption(option)">
				{{ option }}
			</li>
		</ul>
	</div>
</template>
