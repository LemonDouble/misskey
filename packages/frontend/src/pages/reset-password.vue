<!--
SPDX-FileCopyrightText: syuilo and misskey-project
SPDX-License-Identifier: AGPL-3.0-only
-->

<template>
<PageWithHeader :actions="headerActions" :tabs="headerTabs">
	<MkSpacer v-if="token" :contentMax="700" :marginMin="16" :marginMax="32">
		<div class="_gaps_m">
			<MkInput v-model="password" type="password">
				<template #prefix><i class="ti ti-lock"></i></template>
				<template #label>{{ i18n.ts.newPassword }}</template>
			</MkInput>

			<MkButton primary @click="save">{{ i18n.ts.save }}</MkButton>
		</div>
	</MkSpacer>
</PageWithHeader>
</template>

<script lang="ts" setup>
import { defineAsyncComponent, onMounted, ref, computed } from 'vue';
import MkInput from '@/components/MkInput.vue';
import MkButton from '@/components/MkButton.vue';
import * as os from '@/os.js';
import { i18n } from '@/i18n.js';
import { definePage } from '@/page.js';
import { mainRouter } from '@/router.js';

const props = defineProps<{
	token?: string;
}>();

const password = ref('');

async function save() {
	await os.apiWithDialog('reset-password', {
		token: props.token,
		password: password.value,
	});
	mainRouter.push('/');
}

onMounted(() => {
	if (props.token == null) {
		const { dispose } = os.popup(defineAsyncComponent(() => import('@/components/MkForgotPassword.vue')), {}, {
			closed: () => dispose(),
		});
		mainRouter.push('/');
	}
});

const headerActions = computed(() => []);

const headerTabs = computed(() => []);

definePage(() => ({
	title: i18n.ts.resetPassword,
	icon: 'ti ti-lock',
}));
</script>
