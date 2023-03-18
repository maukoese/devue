<script>
	export default {
		name: "todos",
		data: () => ({
			todos: [],
			user: {},
			search: "",
			detailsPopup: {
				id: "todo-details",
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
			const todos = query.title
				? await $axios.$get(
						"https://jsonplaceholder.typicode.com/todos",
						{ params: query }
				  )
				: await $axios.$get(
						"https://jsonplaceholder.typicode.com/todos"
				  );

			return { todos, search };
		},
	};
</script>

<template>
	<b-container>
		<b-row class="p-3">
			<div class="col-md-4">
				<h1>Todos</h1>
			</div>
			<div class="col-md-4 offset-md-4">
				<b-form class="d-flex">
					<b-form-input
						class="mr-sm-2"
						placeholder="Search in todos by title"
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
				<b-table
					:fields="['id', 'title', 'completed']"
					:items="todos"
				>
					<template #cell(title)="row">
						<span
							role="button"
							@click="getInfo(row.item, row.index, $event.target)"
						>
							{{ row.item.title }}
						</span>
					</template>
					<template #cell(completed)="row">
						<b-form-checkbox
							:id="`checkbox-${row.item.id}`"
							v-model="row.item.completed"
							:name="`checkbox-${row.item.id}`"
							unchecked-value="not_accepted"
						>
							{{
								row.item.completed
									? "Completed"
									: "Not Completed"
							}}
						</b-form-checkbox>
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
