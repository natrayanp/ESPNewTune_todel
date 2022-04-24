<!-- App.svelte -->
<script>
	import { Router, Link, Route } from "svelte-routing";
	import Home from "./components/Home.svelte";	  
	import SelectScan from "./components/SelectScan.svelte";
	export let url = "";
  </script>
  
  <Router url="{url}">
	<nav>
	  <Link to="/">Home</Link>
	  <Link to="/cont">scan</Link>
	</nav>
	<div>
		<Route path="/cont"><SelectScan /></Route>
	  	<Route path="/"><Home /></Route>
	</div>
  </Router>