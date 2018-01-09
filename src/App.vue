<template>
	<div id="app">
		<h3>Vuenograms</h3>
		<vuenogram title="Prueba" :nonogram="nonogram"></vuenogram>
	</div>
</template>

<script>
	import Vuenogram from './components/Vuenogram.vue'

	export default {
		components: { Vuenogram },
    name: 'app',
		data() {
			return {
				nonogram: [
					[0,1,1,0,1],
					[0,0,0,0,1],
					[1,0,1,0,1],
					[1,0,1,0,1],
					[0,1,1,1,0]
				]
			}
		}
  }
</script>

<style>
	#app {
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		/*text-align: center;*/
		color: #2c3e50;
		margin-top: 60px;
	}
</style>