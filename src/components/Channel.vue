<template>
	<div>
		<h2 class="c-sound__heading">{{ title }}</h2>

		<!-- controls -->
		<div class="c-sound__item">
			<!-- play pause -->
			<button
				class="c-button"
				:class="{'is-playing': isPlaying}"
				@click="playPause()"
			>{{ playPauseButton }}</button>

			<!-- volume -->
			<button
				class="c-button c-button--round"
				:class="{'is-playing': isPlaying}"
				:disabled="decDisabled"
				@click="changeVolume('-')"
			>–</button>
			<button
				class="c-button c-button--round"
				:class="{'is-playing': isPlaying}"
				:disabled="incDisabled"
				@click="changeVolume('+')"
			>+</button>
			<p class="u-fs-14 u-fw-b">{{ volume.current }}%</p>
			<div class="u-o-hidden">
				<div class="c-sound-vu" ref="vu">
					<div class="c-sound-vu__meter" v-for="n in 16"></div>
				</div>
			</div>
		</div>

		<!-- audio -->
		<audio ref="audio" loop :src="'./src/assets/media/' + file"></audio>
	</div>
</template>

<script>
	import { gsap } from 'gsap';
	export default {
		props: {
			title: String,
			file: String
		},

		data() {
			return {
				volume: {
					current: 100,
					min: 0,
					max: 100,
					increment: 20
				},
				isPlaying: false,
				playPauseButton: 'Play',
				decDisabled: false,
                incDisabled: true,
                tween: Object,
			};
		},

		methods: {
			vuPlaying() {
				const el = this.$refs.vu.children;
				const tween = () => {
					this.tween = gsap.fromTo(el, {
                        scaleY: 'random(0.2, 1)',
						opacity: 'random(0.1, 0.8)'
                    },{
						duration: 0.1,
						delay: 'random(0, 0.08)',
						ease: 'power3.inOut',
						scaleY: 'random(0.2, 1)',
						opacity: 'random(0.1, 0.8)',
						repeat: 1,
						yoyo: true,
						onComplete() {
							// loop animation manually to reset random values every loop
							tween();
						}
                    });
				};
				tween();
			},

			vuPlayPause(opacity, scale) {
				gsap.to(this.$refs.vu, {
					duration: 0.75,
					ease: 'power4.out',
					scaleY: scale,
					autoAlpha: opacity
				});
			},

			vuChangeScale() {
				const scale = this.volume.current / 100;
				gsap.to(this.$refs.vu, {
					duration: 0.3,
					ease: 'power3.out',
					scaleY: scale
				});
			},

			seamlessLoop() {
				const audio = this.$refs.audio;
				const buffer = 3;

				audio.addEventListener('timeupdate', () => {
					if (audio.currentTime > audio.duration - buffer) {
						audio.currentTime = 0;
						audio.play();
					}
				});
			},

			playPause() {
				if (!this.isPlaying) {
					this.$refs.audio.play();
					this.vuPlayPause(1, this.volume.current / 100);
					this.vuPlaying();
					this.playPauseButton = 'Pause';
					this.seamlessLoop();
				} else {
					this.$refs.audio.pause();
					this.vuPlayPause(0, 0);
					setTimeout(() => this.tween.kill(), 1);
					this.playPauseButton = 'Play';
				}
                this.isPlaying = !this.isPlaying;
			},

			checkVolumeButtons() {
				const volume = this.volume.current;

				volume === this.volume.min
					? (this.decDisabled = true)
					: (this.decDisabled = false);

				volume === this.volume.max
					? (this.incDisabled = true)
					: (this.incDisabled = false);
			},

			changeVolume(direction) {
				const audio = this.$refs.audio;
				const min = this.volume.min + this.volume.increment;
				const max = this.volume.max - this.volume.increment;

				if (direction === '-') {
					if (this.volume.current >= min) {
						this.volume.current -= this.volume.increment;
						audio.volume = this.volume.current / 100;
					}
				} else if (direction === '+') {
					if (this.volume.current <= max) {
						this.volume.current += this.volume.increment;
						audio.volume = this.volume.current / 100;
					}
				}

				this.checkVolumeButtons();
				this.vuChangeScale();
			}
		},

		beforeMount() {
			this.checkVolumeButtons();
		}
	};
</script>