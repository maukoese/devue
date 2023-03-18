<script>
	export default {
		name: "posts",
		data: () => ({
			posts: [],
			user: {},
			search: "",
			detailsPopup: {
				id: "post-details",
				title: "",
				content: "",
			},
		}),
		methods: {
			async getInfo(item, index, button) {
				this.user = await this.$axios.$get(
					`https://jsonplaceholder.typicode.com/users/${item.userId}`
				);
				this.detailsPopup.content = this.user?.name;
				this.$root.$emit(
					"bv::show::modal",
					this.detailsPopup.id,
					button
				);
			},
			resetDetailsPopup() {
				this.detailsPopup.content = "";
			},
		},
		async asyncData({ $axios, query }) {
			const search = query.title ?? "";
			const posts = query.title
				? await $axios.$get(
						"https://jsonplaceholder.typicode.com/posts",
						{ params: query }
				  )
				: await $axios.$get(
						"https://jsonplaceholder.typicode.com/posts"
				  );

			return { posts, search };
		},
	};
</script>

<template>
	<b-container>
		<b-row class="p-3">
			<div class="col-md-4">
				<h1>Posts</h1>
			</div>
			<div class="col-md-4 offset-md-4">
				<b-form class="d-flex">
					<b-form-input
						class="mr-sm-2"
						placeholder="Search in posts by title"
						name="title"
						:value="search"
					></b-form-input>
					<b-button
						variant="info"
						class="my-2 my-sm-0"
						type="submit"
					>
						Search
					</b-button>
				</b-form>
			</div>

			<div class="col-md-12">
				<b-table :fields="['id', 'title', 'body']" :items="posts">
					<template #cell(title)="row">
						<span
							role="button"
							@click="getInfo(row.item, row.index, $event.target)"
						>
							{{ row.item.title }}
						</span>
					</template>
					<template #cell(body)="row">
						<span
							role="button"
							@click="getInfo(row.item, row.index, $event.target)"
						>
							{{ row.item.body }}
						</span>
					</template>
				</b-table>

				<b-modal
					:id="detailsPopup.id"
					title="Author"
					ok-only
					centered
					@hide="resetDetailsPopup"
				>
					<pre>{{ detailsPopup.content }}</pre>
				</b-modal>
			</div>
		</b-row>
	</b-container>
</template>
