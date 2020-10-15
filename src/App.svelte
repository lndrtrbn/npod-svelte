<script>
	import { Router, Route, Link } from "svelte-routing";
	import PageA from "./PageA.svelte";
	import PageB from "./PageB.svelte";
	export let url = ""; //This property is necessary declare to avoid ignore the Router
</script>

<main>
	<Router url="{url}">
		<nav>
		   <Link to="/">Home</Link>
		   <Link to="about">About</Link>
		 </nav>
		 <div>
		   <Route path="about" component="{PageB}" /> 
		   <!--for now the router just support case sensitive,
			   one workaround colud be add two time the route
			   Example.
			  <Route path="About" component="{About}" /> 
		   -->
		   <Route path="/"><PageA /></Route>
		 </div>
	   </Router>
</main>

<style lang="scss">
	@import "./styles/variables.scss";

	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
		background-color: $secondary;

		h1 {
			color: $primary;
			text-transform: uppercase;
			font-size: 4em;
			font-weight: 100;
		}
	}


	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>