<template>
<div>
	<div v-if="game.ready" :class="$style.game">
		<div :class="$style.count" class="">큰 버튼을 눌러 커모지를 고치세요!</div>
		<button v-click-anime class="_button" @click="onClick">
			<img src="/client-assets/big-button.png" :class="$style.img">
		</button>
	</div>
	<div v-else>
		<MkLoading/>
	</div>
</div>
</template>

<script lang="ts" setup>
import * as os from '@/os';
import * as game from '@/scripts/clicker-game';
import MkBoomEffect from "@/components/MkBoomEffect.vue";
import {fetchCustomEmojis} from "@/custom-emojis";

async function onClick(ev: MouseEvent) {
	const x = ev.clientX;
	const y = ev.clientY;
	os.popup(MkBoomEffect, { x, y }, {}, 'end');

	await fetchCustomEmojis(true);
	location.reload();
}

</script>

<style lang="scss" module>
.game {
	padding: 16px;
	text-align: center;
}


.count {
	font-weight: 800;
	font-size: 0.875rem;
	margin-bottom: 6px;
}

.img {
	max-width: 90px;
}
</style>
