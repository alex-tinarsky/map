<template>
	<div>
	<ul class="menu cf" v-if="this.menuMode == 0">
		<li><router-link :to="`/`">{{ $t("page.home.title") }}</router-link></li>
		<li><router-link :to="`/about`">{{ $t("page.about.title") }}</router-link></li>
	</ul>
	<ul class="menu cf" v-else>
		<li><router-link :to="`/`" @click.native="clearFileName">{{ $t("page.home.title") }}</router-link></li>
		<li><router-link :to="`/viewPDF?pdfName=${this.$store.getters.getPdfName}&numPages=${this.$store.getters.getNumPages}`">{{ $t("page.viewPDF.title") }}</router-link></li>
		<li>
			<router-link :to="`/viewPDFText?pdfName=${this.$store.getters.getPdfName}`">{{ $t("page.viewText.title") }}</router-link>
			<ul class="submenu">
				<li><router-link :to="`/viewPDFText?pdfName=${this.$store.getters.getPdfName}`">{{ $t("page.viewText.plain") }}</router-link></li>
				<li><router-link :to="`/viewPDFTextSections?pdfName=${this.$store.getters.getPdfName}`" @click.native="InlineButtonClickHandler">{{ $t("page.viewText.highlightSections") }}</router-link></li>
				<li><router-link :to="`/viewPDFTextErrors?pdfName=${this.$store.getters.getPdfName}`">{{ $t("page.viewText.highlightErrors") }}</router-link></li>
			</ul>
		</li>
		<li><router-link :to="`/viewRulesViolations?pdfName=${this.$store.getters.getPdfName}`">{{ $t("page.rulesViolations.title") }}</router-link></li>

	</ul>
	</div>
</template>


<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';

@Component({
	components: {
	},
})

export default class MenuComponent extends Vue {
	menuMode = 0

	clearFileName(): void {
		this.$store.commit('setPdfName', {
			name: ""
		})
	}

	mounted(): void {
		this.$store.commit('setPdfName', {
			name: this.$route.query.pdfName
		})

		let step = 0
		let pdfName = ""
		const checkQuery = () => {
			if (!pdfName) {
				this.$store.commit('setPdfName', {
					name: this.$route.query.pdfName
				})
				pdfName = this.$route.query.pdfName as string;
				this.menuMode = this.$route.query.pdfName ? 1: 0
			}
			if (step < 10) {
				step++;
				setTimeout(checkQuery, Math.pow(2, step) * 100)
			}
		}

		checkQuery()

		console.log(this.menuMode)
	}
}
</script>

<style scoped>
/* Clearing floats */
.cf:before,
.cf:after {
  content: " ";
  display: table;
}

.cf:after {
  clear: both;
}

.cf {
  *zoom: 1;
}

.cf * {
	z-index: 500;
}

/* Mini reset, no margins, paddings or bullets */
.menu,
.submenu {
  margin: 0;
  padding: 0;
  list-style: none;
}

/* Main level */
.menu {
  margin: 50px auto;
  width: 800px;
  /* http://www.red-team-design.com/horizontal-centering-using-css-fit-content-value */
  width: -moz-fit-content;
  width: -webkit-fit-content;
  width: fit-content;
}

.menu > li {
  background: #34495e;
  float: left;
  position: relative;
  transform: skewX(25deg);
}

.menu a {
  display: block;
  color: #fff;
  text-transform: uppercase;
  text-decoration: none;
  font-family: Arial, Helvetica;
  font-size: 17px;
}

.menu li:hover {
  background: #e74c3c;
}

.menu > li > a {
  transform: skewX(-25deg);
  padding: 1em 2em;
}

/* Dropdown */
.submenu {
  position: absolute;
  width: 200px;
  left: 50%; margin-left: -100px;
  transform: skewX(-25deg);
  transform-origin: left top;
}

.submenu li {
  background-color: #34495e;
  position: relative;
  overflow: hidden;
}

.submenu > li > a {
  padding: 1em 2em;
}

.submenu > li::after {
  content: '';
  position: absolute;
  top: -125%;
  height: 100%;
  width: 100%;
  box-shadow: 0 0 50px rgba(0, 0, 0, .9);
}

/* Odd stuff */
.submenu > li:nth-child(odd){
  transform: skewX(-25deg) translateX(0);
}

.submenu > li:nth-child(odd) > a {
  transform: skewX(25deg);
}

.submenu > li:nth-child(odd)::after {
  right: -50%;
  transform: skewX(-25deg) rotate(3deg);
}

/* Even stuff */
.submenu > li:nth-child(even){
  transform: skewX(25deg) translateX(0);
}

.submenu > li:nth-child(even) > a {
  transform: skewX(-25deg);
}

.submenu > li:nth-child(even)::after {
  left: -50%;
  transform: skewX(25deg) rotate(3deg);
}

/* Show dropdown */
.submenu,
.submenu li {
  opacity: 0;
  visibility: hidden;
}

.submenu li {
  transition: .2s ease transform;
}

.menu > li:hover .submenu,
.menu > li:hover .submenu li {
  opacity: 1;
  visibility: visible;
}

.menu > li:hover .submenu li:nth-child(even){
  transform: skewX(25deg) translateX(15px);			
}

.menu > li:hover .submenu li:nth-child(odd){
  transform: skewX(-25deg) translateX(-15px);			
}

</style>