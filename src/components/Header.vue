<template>
	<header class="u-mb-8 u-mb-12@sm u-mb-18@lg">
		<h1 class="o-heading-l u-mb-2 gsap-stag" v-html="title"></h1>
		<p class="o-heading-m gsap-stag">{{ description }}</p>
	</header>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Ambient<br>Sound Mixer',
				description: 'Customize your background noise'
			};
		}
	};
</script>