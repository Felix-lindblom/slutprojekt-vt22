<script>

/* Deck of cards api
https://deckofcardsapi.com/ 
*/

async function getData(){


	const deckUrl = 'https://deckofcardsapi.com/api/deck/new/shuffle/?deck_count=1';

	let resp = await fetch(deckUrl);
	let deckID = await resp.json();
	console.log('Detta är id av din kortlek "' + deckID.deck_id +'"');
	console.log(deckID)
	/* bara testa att dra kort */
	

	/* körs 2 gånger nu */
	let cardDrawn = 'https://deckofcardsapi.com/api/deck/'+deckID.deck_id+'/draw/?count=2'
	let Sresp = await fetch(cardDrawn);
	let card = await Sresp.json();
	console.log(card.cards[0])
	console.log(card.cards[0].value)
	let cardLetter = card.cards[0].value;
}


</script>

<main>
{#await getData()} 

{/await}
	<body class="wrapper">
		<header>
			<img src="../img/fennyJack.png" alt="Fenny jack logo">
			<aside class="VLcon">
				<h2>VL</h2>
				<article>
					<h2>2.0</h2>
					<!-- poäng in här -->
				</article>
				<aside><h3>I</h3></aside>
				<!-- För att förklara vad vl innebär och vad det står för. Vinst och förlust förhållande -->
			</aside>
		</header>
		<main class="pokerTabel">
			<article class="houseArea">
				<figure>
					<img src="../img/fennyJack.png" alt="Fenny jack logo">
					<h1>j</h1>
					<p><em>10</em></p>
				</figure>
				<figure>
					<img src="../img/fennyJack.png" alt="Fenny jack logo">
					<h1>9</h1>
					<p><em>9</em></p>
				</figure>
				
				<!-- huset kort här -->
			</article>
			<aside id="husCounter">
				<!-- <h3></h3> -->
			</aside>
			<article class="playerArea">
				<aside id="playerCounter">
					
				</aside>
				<section id="playerCards">
					<figure>
						<img src="../img/fennyJack.png" alt="Fenny jack logo">
						<h1>5</h1>
						<p><em>5</em></p>
					</figure>
					<figure>
						<img src="../img/fennyJack.png" alt="Fenny jack logo">
						<h1>Q</h1>
						<p><em>10</em></p>
					</figure>
					<figure>
						<img src="../img/fennyJack.png" alt="Fenny jack logo">
						<h1>6</h1>
						<p><em>6</em></p>
					</figure>
				</section>
			</article>
		</main>
	</body>
</main>

<style lang="scss">
	@mixin center{
		display: flex;
		justify-content: center;
		align-items: center;
	}
	@mixin sidesPad($num){
		padding-left: $num;
		padding-right: $num;
	}
	@mixin cardRatio($num,$unit){
		height: 3.5*$num + $unit;
		width: 2.45*$num + $unit;
		/* Rätt förhållnade på kort */
	}

	$headercol: rgb(230, 139, 20) ;
	$vlcol:rgb(100, 25, 5) ;
	$Whittext:rgb(250,250,250);
	$cardback:rgb(255, 250, 235);
	$pokergreen:rgb(53,101,77);
	$greybak:rgb(75, 75, 75);
	
	header{
		height: 10vw;
		background-color: $headercol;
		display: flex;
		justify-content: space-between;
		img{
			@include sidesPad(1vw);
		}
		/* Behövs fixa bättre med storlkar */
		.VLcon{
			@include center() ;
			@include sidesPad(3vw);
			width: 10vw;
			background-color: $vlcol;
			color: $Whittext;
			justify-content: space-between;
			article{
				background-color: $greybak;
				@include sidesPad(1vw)
			
			}
			aside{

			}
		}
	}

	.pokerTabel{
		height: 90vh;
		background-color: $pokergreen;
		/* https://css-tricks.com/how-to-stack-elements-in-css/ för göra mindre skärmar med flera kort funkar */

		.houseArea{
			@include center();
		}

		#playerCards{
			@include center();
		}		
	}

	/* Vill nog byta det till en klass */
	figure{
		background-color: $cardback;
		@include cardRatio(10,rem );
		margin: 2rem;
		@include center();
		justify-content: space-around;
		flex-direction: column;
		border-radius: 1rem;
		img{
			height: 7rem;
			width: auto;
		}
		h1{
			font-size: 4rem;
		}
	}



</style>