<template>
	<Curtain :state="gameState">
		<div class="h-full flex items-end justify-end">
			<div
				class="flex justify-end items-end bg-black transition duration-[2000ms]"
				:class="{
					'bg-opacity-0': gameState != 'loading',
					'bg-opacity-100':
						gameState == 'loading' || gameState == 'initializing',
				}"
			>
				<div class="px-8 py-4 h-full">
					<div class="flex flex-col justify-between h-full">
						<div>
							<h1 class="text-white">Grass Demo</h1>

							<div
								class="transiton-opacity duration-[2000ms] ease-in"
								:class="{
									'opacity-100': gameState == 'paused' || gameState == 'ready',
									'opacity-0': gameState != 'paused',
								}"
							>
								<StartButton class="my-8" :state="gameState" />
								<Instructions :state="gameState" />
								<Settings :state="gameState" />
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
		<LoadingBar :state="gameState" />
	</Curtain>

	<div>
		<MobileControls v-if="isMobile && gameState == 'playing'"></MobileControls>
	</div>
</template>

<script setup>
import { onMounted, reactive, watch } from "vue";
import LoadingBar from "./components/LoadingBar.vue";
import StartButton from "./components/StartButton.vue";
import Curtain from "./components/Curtain.vue";
import Instructions from "./components/Instructions.vue";
import { gameState } from "./game/State";
import Settings from "./components/Settings.vue";
import MobileControls from "./components/MobileControls.vue";
import { useMobile } from "./composables/useMobile";

const game = window.gameInstance;

const { isMobile } = useMobile();

const reload = () => {
	location.reload();
};

onMounted(() => {
	game.init();
});

watch(gameState, () => {
	/**
	 * while playing, prevent pinch-zoom on mobile devices
	 */
	if (gameState.value == "playing") {
		document.body.classList.add("touch-none");
	} else {
		document.body.classList.remove("touch-none");
	}
});
</script>
