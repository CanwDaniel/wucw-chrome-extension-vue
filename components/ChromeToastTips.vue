<template>
	<section id="chromeToastTips" class="toast" :style="{ 'display': showDialog }">
		<span class="close-btn" @click="close">x</span>
		<p class="toast-text" v-html="showToastTips"></p>
		<p class="toast-url">{{ showToastUrl }}</p>
		<QRcodeCpm v-if="showQRcodeCpm" :toastUrl="toastUrl"></QRcodeCpm>
	</section>
</template>

<script lang="ts" setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';
import { storage } from "wxt/storage";
import QRcodeCpm from "./QRCodeCpm.vue";

const emit = defineEmits(['update:modelValue']);

const toastTips = ref();
const toastUrl = ref();
const showQRcodeCpm = ref(false);
const visible = ref(false);
const state = ref();

let unwatch: (() => void) | undefined;

onMounted(() => {
	unwatch = storage.watch('local:tips', async (newValue) => {
		state.value = newValue ?? null;
		if(state.value.msg != "") {
			show(state.value.msg, state.value.url, state.value.showCode);
		}
		if(state.value.msg == "" && state.value.url == "") {
			close();
		}
	});
});

onUnmounted(() => {
	unwatch?.();
});

const showDialog = computed(() => {
	return visible.value ? 'flex' : 'none';
})

const showToastTips = computed(() => {
	return toastTips.value;
})

const showToastUrl = computed(() => {
	return toastUrl.value;
})

const show = (message: string, url: string, showCode: boolean = false) => {
	visible.value = true;
	toastTips.value = message;
	toastUrl.value = url;
	showQRcodeCpm.value = showCode;
}

const close = () => {
	visible.value = false;
	toastTips.value = '';
	toastUrl.value = '';
	storage.setItem("local:tips", {msg: '', url: '', showCode: false});
}

defineExpose({
	show,
	close
})
</script>

<style scoped lang="scss">
#chromeToastTips {
	position: fixed;
	top: 0;
	left: 0;
	z-index: 99999;
	width: 300px;
	min-height: 300px;
	text-align: center;
	color: #fff;
	background-color: rgba(0, 0, 0, 0.5);
	display: flex;
	justify-content: center;
	align-items: center;
	flex-direction: column;
	border-bottom-right-radius: 80px;
	padding: 30px 10px;

	#qrcode {
		width: 180px;
		height: 180px;
		background-color: #fff;
		align-items: center;
		justify-content: center;
		border-radius: 8px;
	}

	.close-btn {
		position: absolute;
		right: 10px;
		top: 10px;
		font-size: 16px;
		width: 20px;
		height: 20px;
		line-height: 20px;
		cursor: pointer;
		transition: transform .4s;
		will-change: scale;
	}

	.close-btn:hover {
		transform: scale(1.1);
	}

	.toast-text,
	.toast-url {
		color: #fff;
		font-size: 14px;
		word-wrap: break-word;
		width: 100%;
		margin-bottom: 8px;
	}
}
</style>