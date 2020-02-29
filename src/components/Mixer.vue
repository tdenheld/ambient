<template>
	<ul class="c-sound u-pb-4">
		<li v-for="(entry, index) in sounds" class="gsap-stag">
			<h2 class="c-sound__heading">{{ entry.title }}</h2>

			<!-- controls -->
			<div class="c-sound__item">
				<!-- play pause -->
				<button
					class="c-button"
					:class="{'is-playing': entry.isPlaying}"
					@click="playPause(index)"
				>{{ entry.playPauseButton }}</button>

				<!-- volume -->
				<button
					class="c-button c-button--round"
					:class="{'is-playing': entry.isPlaying}"
					:disabled="entry.decDisabled"
					@click="changeVolume(index, '-')"
				>–</button>
				<button
					class="c-button c-button--round"
					:class="{'is-playing': entry.isPlaying}"
					:disabled="entry.incDisabled"
					@click="changeVolume(index, '+')"
				>+</button>
				<p class="u-fs-14 u-fw-b">{{ entry.volume }}%</p>
			</div>

			<!-- audio -->
			<audio ref="audio" loop :src="'./src/assets/media/' + entry.file"></audio>
		</li>
	</ul>
</template>

<script>
	export default {
		data() {
			return {
				sounds: [
					{
						title: "Thunderstorm",
						file: "thunderstorm.m4a"
					},
					{
						title: "Montana Lake",
						file: "montana-lake.m4a"
					},
					{
						title: "Spring Meadow",
						file: "spring-meadow.m4a"
					},
					{
						title: "Varadero Beach",
						file: "varadero-beach.m4a"
					}
				],

				volume: {
					min: 0,
					max: 100,
					default: 100,
					increment: 20
				}
			};
		},

		methods: {
			addProps(index) {
				this.$set(this.sounds[index], "volume", this.volume.default);
				this.$set(this.sounds[index], "isPlaying", false);
				this.$set(this.sounds[index], "playPauseButton", "Play");
				this.$set(this.sounds[index], "decDisabled", false);
				this.$set(this.sounds[index], "incDisabled", true);
			},

			seamlessLoop(index) {
				const audio = this.$refs.audio[index];
				const buffer = 3;

				audio.addEventListener("timeupdate", () => {
					if (audio.currentTime > audio.duration - buffer) {
						audio.currentTime = 0;
						audio.play();
					}
				});
			},

			playPause(index) {
				if (!this.sounds[index].isPlaying) {
					this.$refs.audio[index].play();
					this.sounds[index].playPauseButton = "Pause";
					this.seamlessLoop(index);
				} else {
					this.$refs.audio[index].pause();
					this.sounds[index].playPauseButton = "Play";
				}
				this.sounds[index].isPlaying = !this.sounds[index].isPlaying;
			},

			checkVolumeButtons(index) {
				const volume = this.sounds[index].volume;

				if (volume === this.volume.min) {
					this.sounds[index].decDisabled = true;
				} else {
					this.sounds[index].decDisabled = false;
				}

				if (volume === this.volume.max) {
					this.sounds[index].incDisabled = true;
				} else {
					this.sounds[index].incDisabled = false;
				}
			},

			changeVolume(index, direction) {
				const audio = this.$refs.audio[index];

				if (direction === "-") {
					if (
						this.sounds[index].volume >=
						this.volume.min + this.volume.increment
					) {
						this.sounds[index].volume -= this.volume.increment;
						audio.volume = this.sounds[index].volume / 100;
					}
				} else if (direction === "+") {
					if (
						this.sounds[index].volume <=
						this.volume.max - this.volume.increment
					) {
						this.sounds[index].volume += this.volume.increment;
						audio.volume = this.sounds[index].volume / 100;
					}
				}

				this.checkVolumeButtons(index);
			}
		},

		beforeMount() {
			for (let index in this.sounds) {
				this.addProps(index);
				this.checkVolumeButtons(index);
			}
		}
	};
</script>