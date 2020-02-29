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
		</div>

		<!-- audio -->
		<audio ref="audio" loop :src="'./src/assets/media/' + file"></audio>
	</div>
</template>

<script>
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
				playPauseButton: "Play",
				decDisabled: false,
				incDisabled: true
			};
		},

		methods: {
			seamlessLoop() {
				const audio = this.$refs.audio;
				const buffer = 3;

				audio.addEventListener("timeupdate", () => {
					if (audio.currentTime > audio.duration - buffer) {
						audio.currentTime = 0;
						audio.play();
					}
				});
			},

			playPause() {
				if (!this.isPlaying) {
					this.$refs.audio.play();
					this.playPauseButton = "Pause";
					this.seamlessLoop();
				} else {
					this.$refs.audio.pause();
					this.playPauseButton = "Play";
				}
				this.isPlaying = !this.isPlaying;
			},

			checkVolumeButtons() {
				const volume = this.volume.current;

				if (volume === this.volume.min) {
					this.decDisabled = true;
				} else {
					this.decDisabled = false;
				}

				if (volume === this.volume.max) {
					this.incDisabled = true;
				} else {
					this.incDisabled = false;
				}
			},

			changeVolume(direction) {
                const audio = this.$refs.audio;
                const min = this.volume.min + this.volume.increment;
                const max = this.volume.max - this.volume.increment;

				if (direction === "-") {
					if (this.volume.current >= min) {
						this.volume.current -= this.volume.increment;
						audio.volume = this.volume.current / 100;
					}
				} else if (direction === "+") {
					if (this.volume.current <= max) {
						this.volume.current += this.volume.increment;
						audio.volume = this.volume.current / 100;
					}
				}

				this.checkVolumeButtons();
			}
		},

		beforeMount() {
			this.checkVolumeButtons();
		}
	};
</script>