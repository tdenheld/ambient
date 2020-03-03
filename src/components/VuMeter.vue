<template>
	<div class="vu-meter" ref="vu">
		<div class="vu-meter__line" v-for="n in 16"></div>
	</div>
</template>

<script>
	import { gsap } from 'gsap';

	export default {
        data() {
            return {
                tween: Object
            }
        },

		methods: {
			playing() {
                const el = this.$refs.vu.children;
                
				const tween = () => {
					this.tween = gsap.fromTo(el, {
                        scaleY: 'random(0.2, 1)',
                        opacity: 'random(0.1, 0.8)'
                    },{
                        duration: 0.07,
                        ease: 'power3.inOut',
                        scaleY: 'random(0.2, 1)',
                        opacity: 'random(0.1, 0.8)',
                        repeat: 1,
                        yoyo: true,
                        onComplete() {
                            /* loop animation manually to reset
                               random values every loop */
                            tween();
                        }
                    });
                };                
				tween();
			},

			playPause(opacity, scale) {
                const vm = this;

				gsap.to(vm.$refs.vu, {
					duration: 0.75,
					ease: 'power4.out',
					scaleY: scale,
                    autoAlpha: opacity,
                    onComplete() {
                        if (!opacity) vm.tween.kill();
                    }
                });

                if (opacity) vm.playing();
            },


			changeScale(scale) {
				gsap.to(this.$refs.vu, {
					duration: 0.3,
					ease: 'power3.out',
					scaleY: scale
				});
			}
		}
	};
</script>

<style lang="scss" scoped>
    @import '../assets/styles/settings.variables';
    @import '../assets/styles/tools.mixins';
    
    .vu-meter {
        margin-right: 6px;
        display: grid;
        grid-auto-flow: column;
        justify-content: end;
        gap: 2px;
        align-items: center;
        visibility: hidden;
        transform: scaleY(0);
    }

    .vu-meter__line {
        width: 2px;
        height: 24px;
        background-color: $brand;
        opacity: 0;
        transform: scaleY(0);
    }
</style>