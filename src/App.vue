<template>
	<main class="o-main">
		<div class="o-container">
			<vc-header></vc-header>
			<vc-mixer></vc-mixer>
		</div>
	</main>
</template>

<script>
	import { gsap } from "gsap";
	import Header from "./components/Header.vue";
    import Mixer from "./components/Mixer.vue";

	export default {
		components: {
			"vc-header": Header,
            "vc-mixer": Mixer
		},

		methods: {
			animate() {
				gsap.from(".gs-stag", {
					duration: 2,
					ease: "power4.out",
					opacity: 0,
					y: 40,
					stagger: 0.05
				});
			}
        },
        
        mounted() {
            this.animate();
        }
	};
</script>

<style lang="scss">
	@import "/assets/styles/style.scss";
</style>