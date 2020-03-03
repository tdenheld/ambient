<template>
	<ul class="mixer-grid u-pb-4">
		<li v-for="entry in sounds" class="gsap-stag">
			<vc-channel :title="entry.title" :file="entry.file"></vc-channel>
		</li>
	</ul>
</template>

<script>
    import vcChannel from './Channel.vue';

	export default {
		components: {
			vcChannel
		},

		data() {
			return {
				sounds: [
					{
						title: 'Thunderstorm',
						file: 'thunderstorm.m4a'
					},
					{
						title: 'Montana Lake',
						file: 'montana-lake.m4a'
					},
					{
						title: 'Spring Meadow',
						file: 'spring-meadow.m4a'
					},
					{
						title: 'Varadero Beach',
						file: 'varadero-beach.m4a'
					},
					{
						title: 'Balaton Lake',
						file: 'balaton-lake.m4a'
					}
				]
			};
		}
	};
</script>

<style lang="scss" scoped>
    @import '../assets/styles/style.scss';

	.mixer-grid {
        display: grid;
        gap: $gutter;

		@include sm {
			grid-template-columns: repeat(2, 1fr);
		}

		@include lg {
			grid-template-columns: repeat(3, 1fr);
		}

		@include xl {
			grid-template-columns: repeat(4, 1fr);
		}
	}
</style>