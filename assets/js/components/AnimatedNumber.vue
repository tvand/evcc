<template>
	<span />
</template>

<script>
import { CountUp } from "countup.js";
const DURATION = 0.5;

export default {
	name: "AnimatedNumber",
	props: {
		to: { type: Number, default: 0 },
		format: { type: Function, required: true },
		duration: { type: Number, default: DURATION },
	},
	data() {
		return {
			instance: null,
		};
	},
	watch: {
		to(value) {
			this.instance?.update(value);
		},
	},
	mounted() {
		if (this.instance) {
			return;
		}
		this.instance = new CountUp(this.$el, this.to, {
			startVal: this.to,
			formattingFn: this.format,
			duration: this.duration,
			decimalPlaces: 3,
		});
		if (this.instance.error) {
			console.error(this.instance.error);
		}
	},
	unmounted() {
		this.instance = null;
	},
	methods: {
		forceUpdate() {
			this.instance?.reset();
			this.instance?.update(this.to);
		},
	},
};
</script>

<style scoped>
span {
	font-variant-numeric: tabular-nums;
}
</style>
