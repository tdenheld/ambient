<template>
	<div>
		<h2 class="channel-heading">{{ title }}</h2>

		<!-- controls -->
		<div class="channel-strip">
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

            <!-- VU-meter -->
			<div class="u-o-hidden">
				<vc-vu-meter ref="vu"></vc-vu-meter>
			</div>
		</div>

		<!-- audio -->
		<audio ref="audio" loop :src="'./src/assets/media/' + file"></audio>
	</div>
</template>

<script>
	import vcVuMeter from './VuMeter.vue';

	export default {
		props: {
			title: {
				type: String,
				required: true
			},
			file: {
				type: String,
				required: true
			}
		},

		components: {
			vcVuMeter
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
				incDisabled: true
			};
		},

		methods: {
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
					this.$refs.vu.playPause(1, this.volume.current / 100);
					this.playPauseButton = 'Pause';
					this.seamlessLoop();
				} else {
					this.$refs.audio.pause();
					this.$refs.vu.playPause(0, 0);
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
				this.$refs.vu.changeScale(this.volume.current / 100);
			}
		},

		beforeMount() {
			this.checkVolumeButtons();
		}
	};
</script>

<style lang="scss" scoped>
	@import '../assets/styles/style.scss';

	.channel-heading {
		margin-bottom: $quarter-gutter;
		@include font(18px, bold, $lh-title);
	}

	.channel-strip {
		display: grid;
		// grid-auto-flow: column;
		grid-template-columns: repeat(3, max-content) repeat(2, 1fr);
		gap: $quarter-gutter;
		align-items: center;
		justify-content: start;
		padding: $quarter-gutter;
		background-color: $grey-9;
		border-radius: 3px;
	}
</style>