<template>
	<q-layout view="lHh Lpr lFf">
		<q-header elevated>
			<q-toolbar class="bblue">
				<q-btn
					flat
					dense
					round
					icon="menu"
					aria-label="Menu"
					@click="toggleLeftDrawer"
				/>

				<q-toolbar-title> ToDo Windows </q-toolbar-title>

				<div>juanmacano@gmail.com</div>
			</q-toolbar>
		</q-header>

		<q-drawer
			v-model="leftDrawerOpen"
			show-if-above
			bordered
			class="bg-grey-1"
		>
			<q-list>
				<q-item-label header class="text-grey-8"> Menu </q-item-label>

				<EssentialLink
					v-for="link in essentialLinks"
					:key="link.title"
					v-bind="link"
				/>
			</q-list>
		</q-drawer>

		<q-page-container>
			<router-view />
		</q-page-container>
	</q-layout>
</template>

<script>
import EssentialLink from "components/EssentialLink.vue";

const linksList = [
	{
		title: "ToDo",
		icon: "school",
		link: "/",
	},
];

import { defineComponent, ref } from "vue";

export default defineComponent({
	name: "MainLayout",

	components: {
		EssentialLink,
	},

	setup() {
		const leftDrawerOpen = ref(false);

		return {
			essentialLinks: linksList,
			leftDrawerOpen,
			toggleLeftDrawer() {
				leftDrawerOpen.value = !leftDrawerOpen.value;
			},
		};
	},
});
</script>
