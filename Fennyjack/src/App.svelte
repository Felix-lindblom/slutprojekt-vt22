<script>

/* Deck of cards api
https://deckofcardsapi.com/ 
*/

	async function getData(){

		
		const deckUrl = 'https://deckofcardsapi.com/api/deck/new/shuffle/?deck_count=1';

		let resp = await fetch(deckUrl);
		let deckID = await resp.json();
		console.log('Detta är id av din kortlek "' + deckID.deck_id +'"');
		/* console.log(deckID) */
		/* bara testa att dra kort */
		

		/* körs 2 gånger nu, men det är inte en problem med att dealer ändå ska ha 2 dirket. eller vänta dealer ska få sist */
		
			let cardDrawn = 'https://deckofcardsapi.com/api/deck/'+deckID.deck_id+'/draw/?count=2'
			let Sresp = await fetch(cardDrawn);
			let card = await Sresp.json();
			console.log(card.cards)
			
			console.log(card.cards[0].value)
			console.log(card.cards[1].value)
			console.log('taken cards'+ card.cards.length)
			
			
		for (let i=0;i<card.cards.length;i++){
			if(isNaN(card.cards[i].value)===true){
				if (card.cards[i].value === "ACE"){
					card.cards[i].bjValue = 11
				}else{
					card.cards[i].bjValue = 10
				}
			}else{
				card.cards[i].bjValue = card.cards[i].value
			}
			
		}	

		console.log(card)
		
		return card
		
	}


</script>

<main>

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


				{#await getData()} 
					<h1 class="loading">wait for dealers card</h1>
					{:then card}
						{#each card.cards as playcard,i}
							
						<figure>
							<img src="../img/fennyJack.png" alt="Fenny jack logo">
							<h1>{card.cards[i].value}</h1>
							<p><em>
								
								{card.cards[i].bjValue}
							
							</em></p>
						</figure> 

						{/each}
						
					
				{/await}	

				
				<!-- huset kort här -->
			</article>
			<aside id="husCounter">
				<h3>20</h3>
			</aside>
			<article class="playerArea">
				<aside id="playerCounter">
					<h3>19</h3>
				</aside>
				<section id="playerCards">
					{#await getData()} 
						<h1 class="loading">wait for players card</h1>
						{:then card}
							{#each card.cards as playcard,i}
								
							<figure>
								<img src="../img/fennyJack.png" alt="Fenny jack logo">
								<h1>{card.cards[i].value}</h1>
								<p><em>
									
									{card.cards[i].bjValue}
								
								</em></p>
							</figure> 

							{/each}
							
						
					{/await}	
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
	$greytext:rgb(170, 150, 150);
	$gold:rgb(255, 215, 0);
	$browntext:rgb(54, 22, 13) ;
	

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
	.loading{
		font-size: 7rem;
		color: $Whittext;
	}
	.pokerTabel{
		height: 90vh;
		background-color: $pokergreen;
		/* https://css-tricks.com/how-to-stack-elements-in-css/ för göra mindre skärmar med flera kort funkar */

		.houseArea{
			@include center();

		}
		#husCounter{
			@include center();
			background-color: $gold;
			color: $browntext;
			h3{
				font-size: 2rem;
			}
		}

		.playerArea{

			#playerCounter{
				height: 2*4rem;
				width: 2*4rem;
				background-color: $greybak;
				@include center();
				padding-left: 20vw;
				h3{
					font-size: 4rem
				}
			}

			#playerCards{
				@include center();
			}		
		}
}

	/* Vill nog byta det till en klass */
	/* fundera om du istället vill ha vissa element placeras unkit såsom om  fennyjack placeras relativt till top och value i mitten */
	figure{
		background-color: $cardback;
		@include cardRatio(10,rem );
		
		@include center();
		justify-content: space-between;
		flex-direction: column;
		border-radius: 3ex;
		img{
			height: 25%;
			width: auto;
		}
		h1{
			font-size: 600%;
			
		}
		em{
			color: $greytext;
			font-size: 300%;
		}
	}



</style>