<template>
	<div class="star-rating">
		<div v-for="star in stars" class="star-container">
			<svg class="star-svg"
			     :style="[
							{ fill: `url(#gradient${star.raw})`},
							{ width: starWidth },
							{ height: starHeight },
						]">
				<!--TODO - vertical star-->
				<!--<polygon points="9.9, 1.1, 3.3, 21.78, 19.8, 8.58, 0, 8.58, 16.5, 21.78" style="fill-rule:nonzero;"/>-->
				<polygon :points="getStarPoints" style="fill-rule:nonzero;"/>
				<defs>
					<!--id has to be unique to each star fullness(dynamic offset) - it indicates fullness above-->
					<linearGradient :id="`gradient${star.raw}`" >
						<stop id="stop1" :offset="star.percent" stop-opacity="1" :stop-color="getFullFillColor(star)"></stop>
						<stop id="stop2" :offset="star.percent" stop-opacity="0" :stop-color="getFullFillColor(star)"></stop>
						<stop id="stop3" :offset="star.percent" stop-opacity="1" :stop-color="emptyStarColor"></stop>
						<stop id="stop4" offset="100%"          stop-opacity="1" :stop-color="emptyStarColor"></stop>
					</linearGradient>
				</defs>
			</svg>
		</div>
	</div>
</template>

<script>
	export default {
		name: 'stars-rating',
		components: {

		},
		props: ["config"],
		data: function() {
			return {
				stars: [],
				emptyStar: 0,
				fullStar: 1,
				fullStarColor: '#ed8a19',
				emptyStarColor: '#737373',
				totalStars: 5,
				starWidth: 20,
				starHeight: 20
			}
		},
		directives: {},
		computed: {
			getStarPoints: function (){
				let centerX = this.starWidth / 2;
				let centerY = this.starHeight / 2;

				let innerCircleArms = 5; // a 5 arms star

				let innerRadius = this.starWidth / innerCircleArms;
				let innerOuterRadiusRatio = 2.5; // Unique value - determines fatness/sharpness of star
				let outerRadius = innerRadius * innerOuterRadiusRatio;

				return this.calcStarPoints(centerX, centerY, innerCircleArms, innerRadius, outerRadius);
			},
		},
		methods: {
			calcStarPoints(centerX, centerY, innerCircleArms, innerRadius, outerRadius) {
				let angle = (Math.PI / innerCircleArms);
				let angleOffsetToCenterStar = 60;

				let totalArms = innerCircleArms * 2;
				let points = "";
				for (let i = 0; i < totalArms; i++) {
					let isEvenIndex = i % 2 == 0;
					let r = isEvenIndex ? outerRadius : innerRadius;
					let currX = centerX + Math.cos(i * angle + angleOffsetToCenterStar ) * r;
					let currY = centerY + Math.sin(i * angle + angleOffsetToCenterStar) * r;
					points += currX + ',' + currY + ' ';
				}
				return points;
			},
			initStars() {
				for(let i = 0; i < this.totalStars; i++) {
					this.stars.push({
						raw: this.emptyStar,
						percent: this.emptyStar + '%',
					});
				}
			},
			setStars() {
				let fullStarsCounter = Math.floor(this.config.rating);
				for(let i = 0; i < this.stars.length; i++) {
					if (fullStarsCounter !== 0) {
						this.stars[i].raw = this.fullStar;
						this.stars[i].percent = this.calcStarFullness(this.stars[i]);
						fullStarsCounter--;
					} else {
						let rateRemainder = this.totalStars - this.config.rating;
						let roundedOneDecimalPoint = Math.round(rateRemainder * 10) / 10;
						this.stars[i].raw = roundedOneDecimalPoint;
						this.stars[i].percent = this.calcStarFullness(this.stars[i]);
					}
				}
			},
			setConfigData() {
				if (this.config) {
					if (this.config.fullStarColor) {
						this.fullStarColor = this.config.fullStarColor;
					}
					if (this.config.emptyStarColor) {
						this.emptyStarColor = this.config.emptyStarColor;
					}
					if (this.config.starWidth) {
						this.starWidth = this.config.starWidth;
					}
					if (this.config.starHeight) {
						this.starHeight = this.config.starHeight;
					}
				}
			},
			getFullFillColor(starData) {
				return starData.raw !== this.emptyStar ? this.fullStarColor : this.emptyStarColor;
			},
			calcStarFullness(starData) {
				let starFullnessPercent = (starData.raw * 100) + '%';
				return starFullnessPercent;
			}
		},
		created() {
			this.initStars();
			this.setStars();
			this.setConfigData();
		}
	}
</script>

<style scoped lang="scss">
	.star-rating {
		display: flex;
		.star-container {
			display: flex;
			.star-svg {

			}
		}
		.star-container:not(:last-child) {
			margin-right: 5px;
		}
	}
</style>
