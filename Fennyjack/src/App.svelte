<script>

/* Deck of cards api
https://deckofcardsapi.com/ 
*/

	let Adeck = ''
	let numCard = 2
	let delarCard = 2
	/* Sen när du ska imitera mig så sätt wins på 200 */
	let wins = 1
	let loses = 1


	async function getData(num){

		/* ska sova nu men titta om du kan skippa att skapa Adeck där upp och om den skapa i. Annars kan det kräva göra om detta en del */
		if(Adeck.deck_id === undefined ||Adeck.reaming < 0){
			const deckUrl = 'https://deckofcardsapi.com/api/deck/new/shuffle/?deck_count=1';

			let resp = await fetch(deckUrl);
			let deckID = await resp.json();
			console.log('Detta är id av din kortlek "' + deckID.deck_id +'"');
			/* console.log(deckID) */
			/* bara testa att dra kort */
			Adeck = deckID
			/* körs 2 gånger hmmmmm */
			console.log('loop 1')
		}else{
			console.log("2 time")
			console.log()
		}
			let cardDrawn = 'https://deckofcardsapi.com/api/deck/'+Adeck.deck_id+'/draw/?count=' + num
			let Sresp = await fetch(cardDrawn);
			let card = await Sresp.json();
/* 			console.log(card.cards) */
			
/* 			console.log(card.cards[0].value)
			console.log(card.cards[1].value)
			console.log('taken cards'+ card.cards.length) */
			
			card.totP = 0 
			
		for (let i=0;i<card.cards.length;i++){
			if(isNaN(card.cards[i].value)===true){
				if (card.cards[i].value === "ACE"){
					card.cards[i].bjValue = 11
					card.totP += 11 
				}else{
					card.cards[i].bjValue = 10
					card.totP += 10
				}
			}else{
				card.cards[i].bjValue = card.cards[i].value
				card.totP +=  parseInt(card.cards[i].value)	

			}
			
		}	
		console.log(card.totP)
	
		if(card.totP >= 21  ){
			if(card.totP === 21	){
				card.totp = 'blackjack'
			}else{
				card.totP = 'bust at ' + card.totP
			}	
		}
		console.log(card.totP > 21)
		
		return card
		
	}

	function hit(){
		numCard += 1
	}

	function stand(){
		/* vet att dennna inte funkar än men det ska vara något sånt här 
		if (card.totP < 17){
			/hus slåt bara om dom är under 16 eller 17 
			delarCard += 1
		}else{
			console.log('check time')
			/* tittar på spelares card.totp och om den är närmare 21 eller inte 
			
			
		}*/
		console.log('keep standing')
	};



	function Vl(){
		return wins/loses
	}
	function win(){
		wins += 1
	}
	function losted(){
		loses += 1
	}

</script>

<main>

	<body class="wrapper">
		<header>
			<img src="../img/fennyJack.png" alt="Fenny jack logo">
			<aside class="VLcon">
				<h2>VL</h2>
				<article>
					<h2>{Vl()}</h2>
					<!-- poäng in här -->
				</article>
				<aside><h3>I</h3></aside>
				<!-- För att förklara vad vl innebär och vad det står för. Vinst och förlust förhållande -->
			</aside>
		</header>
		<main class="pokerTabel">
			


				{#await getData(delarCard)} 
				<article class="houseArea">
					<h1 class="loading">wait for dealers card</h1>
				</article>	
				<aside id="husCounter">
					<h3>Wait...</h3>		
				</aside>
				{:then card}
						<article class="houseArea">
						
							{#each card.cards as playcard,i}
								
							<figure>
								<img src="../img/fennyJack.png" alt="Fenny jack logo">
								<h1>{card.cards[i].value}</h1>
								<p><em>
									
									{card.cards[i].bjValue}
								
								</em></p>
							</figure> 

							{/each}

						</article>

					<aside id="husCounter">
						<h3>{card.totP}</h3>		
					</aside>

				{/await}	

				
				<!-- huset kort här -->
			
			
			<article class="playerArea">

				
					{#await getData(numCard)} 
					<aside id="playerCounter">
							<h3>wait</h3>
					</aside>
					<section id="playerCards">
						<h1 class="loading">wait for players card</h1>
						
						
					</section>
						{:then card}
							<aside id="playerCounter">
								<h3>{card.totP} </h3>
							</aside>							
						<section id="playerCards">
						
							{#each card.cards as playcard,i}
								
							<figure>
								<img src="../img/fennyJack.png" alt="Fenny jack logo">
								<h1>{card.cards[i].value}</h1>
								<p><em>
									{card.cards[i].bjValue}
								</em></p>
							</figure> 

							{/each}
							<section class="playerButtons">
								<button id="hitB" class="B" on:click={hit} >
									<h3>HIT!</h3>
								</button>
								<button id="standB" class="B" on:click={stand} >
									<h3>Stand</h3>
								</button>
							</section>

						</section>
						
					{/await}	

				
			</article>
		</main>
	</body>
</main>

<style lang="scss">

	/* mixin */
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

	/* colors */
		$headercol: rgb(230, 139, 20) ;
		$vlcol:rgb(100, 25, 5) ;
		$Whittext:rgb(250,250,250);
		$cardback:rgb(255, 250, 235);
		$pokergreen:rgb(53,101,77);
		$greybak:rgb(75, 75, 75);
		$greytext:rgb(170, 150, 150);
		$gold:rgb(255, 215, 0);
		$browntext:rgb(54, 22, 13) ;
		$hit:rgb(14, 176, 2) ;
		$stand:rgb(20, 20, 20) ;
	
	

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
					font-size: 4rem;
					color: $Whittext
				}
			}

			#playerCards{
				@include center();
			}	

			.playerButtons{
				@include center();
				flex-direction: column;
				.B{
					@include cardRatio(2,rem );
					border-radius: 1rem;
					border: 0;
					margin: 1rem;
				}
				#hitB{
					background-color: $hit;
					color: $Whittext;
				}
				#standB{
					background-color: $stand;
					color: $Whittext;
				}
			}

		}
}

	/* Vill nog byta det till en klass */
	/* fundera om du istället vill ha vissa element placeras unkit såsom om  fennyjack placeras relativt till top och value i mitten */
	figure{
		background-color: $cardback;
		@include cardRatio(7,vh );
		
		display: grid;
		grid-template-columns: 1fr;
		grid-template-rows: 1fr 2fr 1fr;
		grid-template-areas: 
		'lo'
		'ty'
		'va'
		;
		@include center();


		justify-content: space-between;
		flex-direction: column;
		border-radius: 1.5vh;
		padding: 2vh;
		img{
			grid-area: lo;
			height: auto;
			width: 20vh;
		}
		h1{
			grid-area: ty;
			font-size: 4vh;
		}
		em{
			grid-area: va;
			color: $greytext;
			font-size: 2vh;
		}
	}



</style>