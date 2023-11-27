<script setup lang="ts">
import type { PropType } from 'vue';
import { useI18n } from '@/composables';

export type CredentialOption = {
	id: string;
	name: string;
	typeDisplayName: string | undefined;
};

const props = defineProps({
	credentialOptions: {
		type: Array as PropType<CredentialOption[]>,
		required: true,
	},
	selectedCredentialId: {
		type: String,
		required: false,
	},
});

const $emit = defineEmits({
	credentialSelected: (_credentialId: string) => true,
	newCredential: () => true,
});

const i18n = useI18n();

const NEW_CREDENTIALS_TEXT = `- ${i18n.baseText('nodeCredentials.createNew')} -`;

const onCredentialSelected = (credentialId: string) => {
	if (credentialId === NEW_CREDENTIALS_TEXT) {
		$emit('newCredential');
	} else {
		$emit('credentialSelected', credentialId);
	}
};
</script>

<template>
	<n8n-select
		size="small"
		:modelValue="props.selectedCredentialId"
		@update:modelValue="onCredentialSelected"
	>
		<n8n-option
			v-for="item in props.credentialOptions"
			:data-test-id="`node-credentials-select-item-${item.id}`"
			:key="item.id"
			:label="item.name"
			:value="item.id"
		>
			<div :class="[$style.credentialOption, 'mt-2xs mb-2xs']">
				<n8n-text bold>{{ item.name }}</n8n-text>
				<n8n-text size="small">{{ item.typeDisplayName }}</n8n-text>
			</div>
		</n8n-option>
		<n8n-option
			data-test-id="node-credentials-select-item-new"
			:key="NEW_CREDENTIALS_TEXT"
			:value="NEW_CREDENTIALS_TEXT"
			:label="NEW_CREDENTIALS_TEXT"
		>
		</n8n-option>
	</n8n-select>
</template>

<style lang="scss" module>
.credentialOption {
	display: flex;
	flex-direction: column;
}
</style>