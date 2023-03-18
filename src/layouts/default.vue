<script>
	import Navbar from "~/components/Navbar.vue";
	export default {
		name: "home",
		components: { Navbar },
	};
</script>

<template>
	<b-container class="p-0" fluid>
		<header>
            <Navbar />
        </header>
		<main>
			<Nuxt />
		</main>

		<footer>
        </footer>
	</b-container>
</template>
