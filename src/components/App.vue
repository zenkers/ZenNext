<template>
	<div id="app">
		<zen-next :visible="modalVisible"
				  :modalContent="modalContent"
				  @modalVisible="modalVisible = $event">
		</zen-next>
		<div class="container"></div>
	</div>
</template>

<script>
	import ZenNext from './ZenNext.vue'
	export default {
		name: 'app',
		components: {
			'zen-next' : ZenNext
		},
		data() {
			return {
				modalVisible: false,
				modalContent: [{
					'title': 'Welcome to the ZenNext Demo',
					'content': 'ZenNext makes it pretty simple to do some pretty sweet stuff.'
					}, {
					'title': 'ZenNext divides an array of json data into individual windows.',
					'content': 'test4'
					}, {
					'title': 'Customizing ZenNext windows is simple.',
					'content': 'Customizing ZenNext is as easy as adding the properties to this object, and the outputs to the ZenNext component.'
					}]
			}
		},
		// Reveals the modal after the given time.
		mounted() {
			setTimeout( () => {
				this.modalVisible = !this.modalVisible
			}, 1000)
		}
	}
</script>

<style lang="scss">
	#app {
		.container {
			margin: 0 auto;
			width: 1024px;
		}
	}
</style>
