<script>
	import { gsap } from 'gsap';

	export default {
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
    .gsap-stag {
        visibility: hidden;
    }
</style>