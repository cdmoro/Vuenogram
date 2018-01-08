<template>
	<section id="nonogram">
		<div id="nnWrapper">
			<div>
				<div class="row" v-for="(col, i) in nnCols[0].length" :key="i">
					<div class="col cell nnRow nnRowEmpty" v-for="i in nnRows[0].length">&nbsp;</div>
					<div class="col cell nnRow" v-for="(cell, j) in nnCols" :key="j">
					<small>{{ nnCols[j][i] ? nnCols[j][i] : '&nbsp;' }}</small>
					</div>
				</div>
				<div class="row" v-for="(row, i) in nonogram" :key="i">
					<div class="col cell nnRow" v-for="(nnRow, h) in nnRows[i]" :key="h">
						<small>{{ nnRow ? nnRow : '&nbsp;' }}</small>
					</div>
					<square :coords="[i, j]" v-for="(cell, j) in row" :key="j" @clickedSquare="updateNn"></square>
				</div>
				<div><small>{{ nnSize }}</small></div>
			</div>
		</div>
	</section>
</template>

<script>
  import square from './Square.vue'
	import compact from 'lodash/compact'
	import flatten from 'lodash/flatten'

	export default {
  name: 'Vuenogram',
	components: { square },
	props: {
		nonogram: {
			type: Array,
			required: true
		}
	},
  data () {
    return {
			progress: []
    }
  },
	created () {
		let nnLength = this.nonogram[0].length

		for (let row of this.nonogram)
			this.progress.push(Array(nnLength).fill(0))
		
	},
	methods: {
		updateNn(clicked, coords) {
			this.progress[coords[0]][coords[1]] = clicked ? 1 : 0
		}
	},
	computed: {
		countProgress() {
			return compact(flatten(this.progress)).length
		},
		nnSize() {
			return this.nonogram.length + "x" + this.nonogram[0].length
		},
		nnRows() {
			let aux = []
			let rows = 0
			for(let i = 0; i < this.nonogram.length; i++) {
				let arrSum = []
				let sum = 0
				let rowsAnt = 0
				for(let j = 0; j < this.nonogram[i].length; j++) {
					if (this.nonogram[i][j] == 1) {
						sum++
					} else if (sum != 0) {
						arrSum.push(sum)
						rowsAnt++
						sum = 0
						if (rows < rowsAnt) rows = rowsAnt
					}
					if (j == (this.nonogram[i].length - 1) && sum != 0) {
						arrSum.push(sum)
						rowsAnt++
						if (rows < rowsAnt) rows = rowsAnt
					}
				}
				aux.push(arrSum)
			}
			for (let i = 0; i < aux.length; i++) {
				if (aux[i].length < rows) {
					for(let j = 0; j < (rows - aux[i].length + 1); j++) {
						aux[i].unshift(0)
					}
				}
			}
			return aux
		},
		nnCols() {
			let aux = []
			let cols = 0
			for (let i = 0; i < this.nonogram[0].length; i++) {
				let arrSum = []
				let sum = 0
				let colsAnt = 0
				for (let j = 0; j < this.nonogram.length; j++) {
					if (this.nonogram[j][i] == 1) {
						sum++
					} else if (sum != 0) {
						arrSum.push(sum)
						colsAnt++
						sum = 0
						if (cols < colsAnt) cols = colsAnt
					}
					if (j == (this.nonogram[i].length - 1) && sum != 0) {
						arrSum.push(sum)
						colsAnt++
						if (cols < colsAnt) cols = colsAnt
					}
				}
				aux.push(arrSum)
			}

			for (let i = 0; i < aux.length; i++) {
				if (aux[i].length < cols) {
					for(let j = 0; j < ((cols - aux[i].length + 1)); j++) {
						aux[i].unshift(0)
					}
				}
			}

			return aux
		}
	}
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

#nonogram {
	margin: 0 auto;
	
}

#nnWrapper {
	display: inline-block;
	margin: 0 auto;
}

*, ::after, ::before {
	box-sizing: border-box;
}

.row {
	
}

.cell {
	width: 30px !important;
	height: 30px;
	display: inline-block;
	border: #DDD solid 1px;
	margin: 1px;
	border-radius: 2px;
	text-align: center;

	-webkit-touch-callout: none; /* iOS Safari */
	-webkit-user-select: none; /* Safari */
	-khtml-user-select: none; /* Konqueror HTML */
	-moz-user-select: none; /* Firefox */
	-ms-user-select: none; /* Internet Explorer/Edge */
	user-select: none;
}

.nnRow {
	background-color: #35495E;
	border-color: #35495E;
	color: #DDD;
}
.nnRow.nnRowEmpty {
	opacity: .8
}
</style>