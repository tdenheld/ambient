<template>
	<ul class="mixer-grid u-pb-4">
		<li v-for="entry in sounds" class="a a-fi-up-small a-stag-del a-dur-1500">
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
					},
                    {
						title: 'Fireplace',
						file: 'fireplace.m4a'
                    },
                    {
						title: 'Evening Lagoon',
						file: 'evening-lagoon.m4a'
                    },
                    {
						title: 'Forrest Rain',
						file: 'forrest-rain.m4a'
                    },
                    {
						title: 'Great Plains',
						file: 'great-plains.m4a'
                    },
                    {
						title: 'Monsoon',
						file: 'monsoon.m4a'
                    },
                    {
						title: 'Night Crickets',
						file: 'night-crickets.m4a'
                    },
                    {
						title: 'Pike Creek',
						file: 'pike-creek.m4a'
					},
				]
			};
		}
	};
</script>

<style lang="scss" scoped>
    @import '../assets/styles/settings.variables';
    @import '../assets/styles/tools.mixins';

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