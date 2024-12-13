<script>
	import { Router, Route } from "svelte-routing";
	import AdPage from "./routes/AdPage.svelte";
</script>

<Router>
	<main>
		<Route path="*" component={AdPage} />
	</main>
</Router>
