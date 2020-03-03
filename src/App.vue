<template>
	<main class="o-main">
		<div class="o-container">
			<vc-header></vc-header>
			<vc-mixer></vc-mixer>
		</div>
	</main>
</template>

<script>
	import { gsap } from 'gsap';
	import vcHeader from './components/Header.vue';
    import vcMixer from './components/Mixer.vue';

	export default {
		components: {
			vcHeader,
            vcMixer
		},

		methods: {
			animate() {
				gsap.fromTo('.gsap-stag', {
					y: 40,
                },{
                    duration: 2,
					ease: 'power4.out',
					autoAlpha: 1,
					y: 0,
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
    
    .gsap-stag {
        visibility: hidden;
    }
</style>