<script>
export default{
    name: 'nav-bar'
}
</script>

<template>
	<div>
		<b-navbar toggleable="lg" type="dark" variant="info">
			<b-navbar-brand href="#">ToDo List App</b-navbar-brand>

			<b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

			<b-collapse id="nav-collapse" is-nav>
				<b-navbar-nav>
				</b-navbar-nav>

				<!-- Right aligned nav items -->
				<b-navbar-nav class="ml-auto me-2">
					<b-nav-item :active="$route.path === '/posts'" href="/posts">Posts</b-nav-item>
					<b-nav-item :active="$route.path === '/todos'" href="/todos" >Todos</b-nav-item>
				</b-navbar-nav>
			</b-collapse>
		</b-navbar>
	</div>
</template>
