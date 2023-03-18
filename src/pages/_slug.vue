<script>
	export default {
		async asyncData({ params }) {
			const slug = params.slug;
			return { slug };
		},
	};
</script>

<template>
    <b-container class="px-auto text-center pt-5">
	<h1 class="align-center m-auto mt-5 pt-5"><em>{{ slug }}</em> Not Found</h1>
    <h3>The page does not exist. go back home</h3>

    <b-link class="btn btn-info btn-lg" href="/">Home</b-link>
    </b-container>
</template>
