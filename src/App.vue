<template>
	<main class="o-main">
		<div class="o-container">
			<vc-header></vc-header>
			<vc-mixer></vc-mixer>
		</div>
        <vc-gsap></vc-gsap>
	</main>
</template>

<script>
	import vcHeader from './components/Header.vue';
    import vcMixer from './components/Mixer.vue';
    import vcGsap from './components/Gsap.vue';

	export default {
		components: {
			vcHeader,
            vcMixer,
            vcGsap
		}
	};
</script>

<style lang="scss">
    @import "/assets/styles/style.scss";
</style>