<script>
	import Cursor from "./core/components/cursor/cursor.svelte";
	import Header from "./core/components/header/header.svelte";
	import { Router, Route } from "svelte-routing";
	import PageA from "./PageA.svelte";
	import PageB from "./PageB.svelte";
	
	export let url = "apod/today";
</script>

<Cursor></Cursor>
<Router url="{url}">
	<Header></Header>
	<Route path="apod/today" component="{PageA}" />
    <Route path="apod/all-pictures" component="{PageB}" />
    <Route path="about" component="{PageA}" />
</Router>


<style lang="scss" global>
	@import url("https://fonts.googleapis.com/css?family=News+Cycle&display=swap");
	@import url('https://fonts.googleapis.com/css2?family=Raleway:wght@900&display=swap');

	body {
		--color-text:       rgb(255, 255, 255);
		--color-text-half:  rgba(255, 255, 255, 0.5);
		--color-bg:         rgb(23, 23, 23);
		--color-primary:    rgb(45, 218, 184);
		--color-secondary:  rgb(212, 7, 73);
		--color-shadow:     rgba(0, 0, 0, 0.75);
	}

	html, body {
		margin: 0;
		height: 100%;
		color: var(--color-text);
		background-color: var(--color-bg);
		font-family: "News Cycle", sans-serif;
	}

	* {
		cursor: none;
	}
</style>