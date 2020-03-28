<template>
	<main class="o-main">
		<div class="o-container">
			<vc-header></vc-header>
			<vc-mixer></vc-mixer>
		</div>
	</main>
</template>

<script>
	import vcHeader from './components/Header.vue';
    import vcMixer from './components/Mixer.vue';

	export default {
		components: {
			vcHeader,
            vcMixer
		}
	};
</script>

<style lang="scss">
    @import "/assets/styles/style.scss";
</style>