<script setup lang="ts">
import { ref } from "vue";
import {
	TransitionRoot,
	TransitionChild,
	Dialog,
	DialogPanel,
	DialogTitle,
} from "@headlessui/vue";
import { menu } from "../data/settings";

const isOpen = ref(false);

function closeModal() {
	isOpen.value = false;
}
function openModal() {
	isOpen.value = true;
}
</script>

<template>
	<button type="button" @click="openModal" class="menuTrigger">Menu</button>
	<TransitionRoot appear :show="isOpen" as="template">
		<Dialog as="div" @close="closeModal" class="dialog">
			<TransitionChild
				as="template"
				enter="enter"
				enter-from="enter-from"
				enter-to="enter-to"
				leave="leave"
				leave-from="leave-from"
				leave-to="leave-to"
			>
				<div class="backdrop" />
			</TransitionChild>

			<div class="panel-grandparent">
				<div class="panel-parent">
					<TransitionChild
						as="template"
						enter="slide-left-enter"
						enter-from="slide-left-enter-from"
						enter-to="slide-left-enter-to"
						leave="slide-left-leave"
						leave-from="slide-left-leave-from"
						leave-to="slide-left-leave-to"
					>
						<DialogPanel class="dialog-panel">
							<DialogTitle as="h3" class="sr-only">
								Menu
							</DialogTitle>
							<ul class="menu--mobile list-reset">
								<li v-for="item in menu">
									<a :href="item.link">{{ item.name }}</a>
								</li>
							</ul>
						</DialogPanel>
					</TransitionChild>
				</div>
			</div>
		</Dialog>
	</TransitionRoot>

	<ul class="menu--desktop list-reset">
		<li v-for="item in menu">
			<a :href="item.link">{{ item.name }}</a>
		</li>
	</ul>
</template>

<style lang="scss">
@use "../sass/abstracts" as *;

.dialog {
	position: relative;
	z-index: 10;
}
.dialog-panel {
	max-width: 80%;
	min-height: 100vh;
	width: 100%;
	background-color: var(--surface1);
	padding: 1rem;
	transition: all;
	overflow: hidden;
	color: var(--black);
	transition: all;
}
.backdrop {
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	background-color: rgba(0, 0, 0, 0.5);
}
.panel-grandparent {
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	overflow-y: auto;
}
.panel-parent {
	display: flex;
	align-items: flex-start;
	justify-content: flex-end;
	min-height: 100%;
}

.menu--mobile {
	display: flex;
	flex-direction: column;
	gap: 0.5rem;
}

.menu--desktop {
	display: none;

	@include min(md) {
		display: flex;
		gap: 1rem;
	}
}

a {
	text-decoration: none;
}

/* Fade In */
.enter-to,
.leave-from {
	opacity: 1;
}

.enter-from,
.leave-to {
	opacity: 0;
}

.enter {
	transition: all 0.3s ease-out;
}

.leave {
	transition: all 0.2s ease-in;
}

/* Slide Left */
.slide-left-enter-to,
.slide-left-leave-from {
	opacity: 1;
	transform: translateX(0);
}

.slide-left-enter-from,
.slide-left-leave-to {
	opacity: 0;
	transform: translateX(100%);
}

.slide-left-enter {
	transition: all 0.3s ease-out;
}

.slide-left-leave {
	transition: all 0.2s ease-in;
}

.menuTrigger {
	@include min(md) {
		display: none;
	}
}
</style>
