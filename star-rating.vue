<template>
	<div class="star-rating">
		<div v-for="star in stars" class="star-container">
			<svg class="star-svg"
			     :style="[
							{ fill: `url(#gradient${star.raw})`},
							{ width: style.starWidth },
							{ height: style.starHeight },
						]">
				<!--TODO - vertical star-->
				<!--<polygon points="9.9, 1.1, 3.3, 21.78, 19.8, 8.58, 0, 8.58, 16.5, 21.78" style="fill-rule:nonzero;"/>-->
				<polygon :points="getStarPoints" style="fill-rule:nonzero;"/>
				<defs>
					<!--id has to be unique to each star fullness(dynamic offset) - it indicates fullness above-->
					<linearGradient :id="`gradient${star.raw}`" >
						<stop id="stop1" :offset="star.percent" stop-opacity="1" :stop-color="getFullFillColor(star)"></stop>
						<stop id="stop2" :offset="star.percent" stop-opacity="0" :stop-color="getFullFillColor(star)"></stop>
						<stop id="stop3" :offset="star.percent" stop-opacity="1" :stop-color="style.emptyStarColor"></stop>
						<stop id="stop4" offset="100%"          stop-opacity="1" :stop-color="style.emptyStarColor"></stop>
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
				totalStars: 5,
				rating: 1,
				style: {
					fullStarColor: '#ed8a19',
					emptyStarColor: '#737373',
					starWidth: 20,
					starHeight: 20
				},
			}
		},
		directives: {},
		computed: {
			getStarPoints: function (){
				let centerX = this.style.starWidth / 2;
				let centerY = this.style.starHeight / 2;

				let innerCircleArms = 5; // a 5 arms star

				let innerRadius = this.style.starWidth / innerCircleArms;
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
				let surplus;
				if (this.config && this.config.rating) {
					this.rating = this.config.rating;
					surplus = this.rating % 1; // Support just one decimal - 2 lines
				} else {
					surplus = 0;
				}
				let fullStarsCounter = Math.floor(this.rating);
				for(let i = 0; i < this.stars.length; i++) {
					if (fullStarsCounter !== 0) {
						this.stars[i].raw = this.fullStar;
						this.stars[i].percent = this.calcStarFullness(this.stars[i]);
						fullStarsCounter--;
					} else {
						let roundedOneDecimalPoint = Math.round(surplus * 10) / 10;
						this.stars[i].raw = roundedOneDecimalPoint;
						return (this.stars[i].percent = this.calcStarFullness(this.stars[i]));
					}
				}
			},
			setConfigData() {
				if (this.config) {
					if (this.config.rating) {
						this.rating = this.config.rating;
					}
					this.setBindedProp(this.style, this.config.style, 'fullStarColor');
					this.setBindedProp(this.style, this.config.style, 'emptyStarColor');
					this.setBindedProp(this.style, this.config.style, 'starWidth');
					this.setBindedProp(this.style, this.config.style, 'starHeight');
				}
			},
			getFullFillColor(starData) {
				return starData.raw !== this.emptyStar ? this.style.fullStarColor : this.style.emptyStarColor;
			},
			calcStarFullness(starData) {
				let starFullnessPercent = (starData.raw * 100) + '%';
				return starFullnessPercent;
			},
			setBindedProp(localProp, propParent, propToBind) {
				if (propParent[propToBind]) {
					localProp[propToBind] = propParent[propToBind];
				}
			},
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
