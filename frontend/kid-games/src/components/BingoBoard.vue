<template>
  <div class="main-content">
    <div class="bingo-card">
      <div class="bingo-card__item"
        v-for="index in boardCells" :key="index"
        @click="activeCell(index)"
        :class="{active: selectedCells.includes(index)}"
      >
        {{(index + 1)}}
        <span class="bingo-card__checkbox"></span>
      </div>
    </div>
  </div>

</template>

<script setup>
import { ref, computed } from 'vue'
const props = defineProps({
  boardSize: {
    type: Number,
    required: true,
    default: 5
  }
})

const selectedCells = ref([])

const boardCells = computed(() => {
  return shuffle(Array.from(Array(props.boardSize * props.boardSize).keys()))
})

function shuffle(array) {
  let currentIndex = array.length,  randomIndex;

  // While there remain elements to shuffle.
  while (currentIndex != 0) {

    // Pick a remaining element.
    randomIndex = Math.floor(Math.random() * currentIndex);
    currentIndex--;

    // And swap it with the current element.
    [array[currentIndex], array[randomIndex]] = [
      array[randomIndex], array[currentIndex]];
  }

  return array;
}

function activeCell(idx){
  selectedCells.value.push(idx)
}
</script>
<style lang="scss" scoped>
$yellow-light: #f9f59e;
$red: #ef3158;
$yellow-dark: #f7d75c;
$orange: #fdb90b;
$orange-dark: #f2ae00;
$brown: #dd8a34;

* {
	box-sizing: border-box;
}

body {
	background: #f8f8f8;
	font-family: "Nunito", sans-serif;
}

aside.context {
	text-align: center;
	color: #333;
	line-height: 1.7;
	a {
		text-decoration: none;
		color: #333;
		padding: 3px 0;
		border-bottom: 1px dashed;
		&:hover {
			border-bottom: 1px solid;
		}
	}
	.explanation {
		max-width: 700px;
		margin: 4em auto 0;
	}
}

footer {
	text-align: center;
	margin: 3em auto;
	width: 100%;
	a {
		text-decoration: none;
		display: inline-block;
		width: 45px;
		height: 45px;
		border-radius: 50%;
		background: transparent;
		border: 1px dashed #333;
		color: #333;
		margin: 5px;
		&:hover {
			background: rgba(255, 255, 255, 0.1);
		}
		.icons {
			margin-top: 12px;
			display: inline-block;
			font-size: 20px;
		}
	}
}

.main-content {
	width: 600px;
	// margin: 4em auto 0;
	overflow: hidden;
}

.title {
	background: $orange;
	color: #fff;
	padding: 30px 10px;
	grid-column: span 5;
	text-align: center;
	font: 72px/0.9 "Amatic SC", cursive;
	span {
		display: none;
		&:nth-child(1) {
			display: block;
		}
		@media screen and (max-width: 600px) {
			&:nth-child(1) {
				display: none;
			}
			&:nth-child(2) {
				display: block;
			}
		}
	}
}

.bingo-card {
	background: $yellow-dark;
	padding: 10px;
	display: grid;
	grid-gap: 3px;
	grid-template-rows: repeat(5, 110px);
	grid-template-columns: repeat(5, 1fr);
	text-transform: uppercase;
	&__item {
		background: $yellow-light;
		display: flex;
		align-items: center;
		text-align: center;
		justify-content: center;
		position: relative;
		cursor: pointer;
    font-size: 45px;
    font-weight: bold;
		line-height: 1.35;
		user-select: none;
		&:after {
			content: "-";
			position: absolute;
			// top: -28%;
			// left: -30%;
			color: $red;
			width: 90%;
			opacity: 0;
			transition: 0.1s ease;
			height: 0;
			pointer-events: none;
			// font: 280px/0.5 "Caveat Brush", cursive;
			text-align: center;
			// transform: rotate(-45deg);
		}
		&.active:after {
			height: 90%;
			opacity: 0.7;
      background-color: $red;
      border-radius: 50%;
		}
	}
}

.bingo-card {
	&__item {
		padding: 15px;
		&.active .bingo-card__checkbox:before {
			content: "\2714";
			position: absolute;
			color: black;
			left: 0;
			top: -19px;
			color: $orange;
			font: 30px "Caveat Brush", cursive;
		}
	}
	&__checkbox {
		display: none;
	}
}

.clear-button {
	margin: 2em 0 0;
	font: 700 16px 'Nunito', sans-serif;
	text-transform: uppercase;
	cursor: pointer;
	display: inline-block;
	border: 2px dotted;
	color: $orange;
	padding: 8px 10px;
	&:hover {
		color: $orange-dark;
	}
}

@media screen and (max-width: 600px) {
	.main-content {
    width: auto;
		max-width: none;
		margin: 0;
	}
	.title {
		font: 50px/0.9 "Amatic SC", cursive;
		padding: 20px;
	}
	.bingo-card {
		// grid-template-rows: repeat(5, 60px);
	  grid-template-rows: repeat(5, 1fr);
    grid-template-columns: repeat(5, 1fr);
		margin: 1em;
    &__item {
      font-size: 30px;
    }
	}
}

</style>
