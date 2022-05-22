<script>

/* Deck of cards api
https://deckofcardsapi.com/ 
*/
	let cardHolder = ''
	let numCard = 2
	let delarCard = 2
	/* Sen när du ska imitera mig så sätt wins på 200 */
	let wins = 1
	let loses = 1
	let score = 0
	let houseval = 0
	let playerval = 0

	/* Ser dumt ut men annars kommer inte kort updateras om det är reda 2 */
	function refreshCard(){
		numCard = 0
		delarCard = 0
		numCard = 2
		delarCard = 2
	}

	async function getData(num,owner){

			const deckUrl = 'https://deckofcardsapi.com/api/deck/new/shuffle/?deck_count=1';

			let resp = await fetch(deckUrl);
			let deckID = await resp.json();
			/* console.log('Detta är id av din kortlek "' + deckID.deck_id +'"'); */
			cardHolder = deckID
	
			let cardDrawn = 'https://deckofcardsapi.com/api/deck/'+cardHolder.deck_id+'/draw/?count=' + num
			let Sresp = await fetch(cardDrawn);
			let card = await Sresp.json();
			/* console.log(card.cards) 
			console.log(card.cards[0].value)
			console.log(card.cards[1].value)
			console.log('taken cards'+ card.cards.length) */
			
			card.totP = 0 
			


		for (let i=0;i<card.cards.length;i++){
			if(isNaN(card.cards[i].value)===true){
				if (card.cards[i].value === "ACE"){
/* 					console.log('ace incoming '+card.totP)
					console.log(card.totP > 10) */
					if (card.totP > 10){
						card.cards[i].bjValue = 1
						card.totP += 1
					}else{
						card.cards[i].bjValue = 11
						card.totP += 11 	
					}
					
				}else{
					card.cards[i].bjValue = 10
					card.totP += 10
				}
			}else{
				card.cards[i].bjValue = card.cards[i].value
				card.totP +=  parseInt(card.cards[i].value)	

			}
			
		}	

		if(owner === 'Dealer'){
			if(card.totP >= 21  ){
				if(card.totP === 21	){
					lost();
					Wl();
					refreshCard();
					console.log('Dealer Won')

				}else{
					card.totP = 'bust at ' + card.totP
					win();
					Wl();
					refreshCard();
					console.log('Dealer bust')

				}	

			
			}
			houseval = card.totP

			


		}

		if(owner === 'Player'){
			playerval = card.totP
		}

		if(card.totP >= 21  ){
					if(card.totP === 21	){
						win();
						Wl();
						refreshCard();
						console.log('Win')

				}else{
					card.totP = 'bust at ' + card.totP
					lost();
					Wl();
					refreshCard();
					console.log('lost')
				}	

				
			}

		

		
		return card
		
	}

	function hit(){
		numCard += 1
	}

	function stand(){
/* 		console.log(houseval + ' House')
		console.log(playerval + ' Player') */
		console.log('keep standing ')

		if(houseval<playerval){
			delarCard += 1
		}else if(houseval===playerval){
			win()
			lost()
			Wl()
			console.log('tie')
			refreshCard();
		}else if(houseval>playerval){
			lost()
			Wl()
			console.log('just dumb, you had nothing to loss')
			refreshCard();
		}
	};



	function Wl(){
		let num = wins/loses
		score = (Math.round(num * 100) / 100).toFixed(2);
		
	}
	function win(){
		wins += 1
	}
	function lost(){
		loses += 1
	}

	function alertRegler(){
		alert('Fennyjack är snarlikt till vanling blackjack på flera sätt. Men det som är unkit är att när du väljer hit så kommer du få ett till kort och dom fördetta blir nya värden. Om du väljer stand så kommer huset välja hit tills det blir mer eller den bustar. Om det är lika värde och du väljer stand så är det en tie och varken + eller - på dina vinster. Hur bra det går ser man på WL(Wins and Loses) som tar ditt snitt på vinst och förlust, som KD. I consolen logas det en del om hur spelet går om du är nyfiken')
	}

</script>

<main>

	<body class="wrapper">
		<header>
			<img src="../img/fennyJack.svg" alt="Fenny jack logo">
			<button id="rules" on:click={alertRegler} >
				<h2 >Regler</h2>
			</button>
			<aside class="WLcon">
				<h2>WL</h2>
				<article>
					<h2>{score}</h2>
				</article>
			</aside>
		</header>
		<main class="pokerTabel">
			{#await getData(delarCard,'Dealer')} 
				<article class="houseArea">
					<figure class="displayCard">
						<h1 class="loading">wait for dealers card</h1>
					</figure>
					
				</article>	
				<aside id="husCounter">
					<h3>Wait...</h3>		
				</aside>
				{:then card}
						<article class="houseArea">
						
							{#each card.cards as playcard,i}
								
							<figure class="displayCard">
								<img src="../img/fennyJack.svg" alt="Fenny jack logo">
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
	
			<article class="playerArea">

				
					{#await getData(numCard,'Player')} 
					<aside id="playerCounter">
							<h3>wait</h3>
					</aside>
					<section id="playerCards">

						<figure class="displayCard">
							<h1 class="loading">wait for playercards</h1>
						</figure>
						
					</section>
						{:then card}
							<aside id="playerCounter">
								<h3>{card.totP} </h3>
							</aside>							
						<section id="playerCards">
						
							{#each card.cards as playcard,i}
								
							<figure class="displayCard">
								<img src="../img/fennyJack.svg" alt="Fenny jack logo">
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
		$Wlcol:rgb(100, 25, 5) ;
		$rgcol:rgb(49, 45, 45);
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
		height: 7vh;
		width: 100vw;
		background-color: $headercol;
		display: flex;
		justify-content: space-between;
		
		
		#rules{
			background-color: $rgcol;
			@include cardRatio(2,vh );
			border-radius: 0.5vh;
			h2{
				
				font-size: 1rem;
				color: $Whittext;
				margin: 0%
			}
		}
		img{
			@include sidesPad(1vw);
		}
		.WLcon{
			@include center() ;
			@include sidesPad(3vw);
			width: 10vw;
			background-color: $Wlcol;
			color: $Whittext;
			justify-content: space-between;
			article{
				background-color: $greybak;
				@include sidesPad(1vw);
				height:100% ;
				margin: 0%;
				@include center();
			}

		}
	}
	.loading{
		font-size: 3vh;
		color: black;
	}
	.pokerTabel{
		height: 93vh;
		width: 100vw;
		background-color: $pokergreen;
		@include center();
		flex-direction: column;

		.houseArea{
			@include center();

		}
		#husCounter{
			@include center();
			background-color: $gold;
			color: $browntext;
			width: 100vw;
			h3{
				font-size: 2rem;
			}
		}

		.playerArea{

			#playerCounter{
				height: 2*4rem;
				width: 100vw;
				background-color: $greybak;
				@include center();
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
					@include cardRatio(2,vh );
					border-radius: 1vh;
					border: 0;
					margin: 1vh;
				}
				#hitB{
					display: flex;
					background-color: $hit;
					color: $Whittext;
					font-size: 1.25rem;
				}
				#standB{
					background-color: $stand;
					color: $Whittext;
					font-size: 1rem;
				}
			}

		}
}


	.displayCard{
		background-color: $cardback;
		@include cardRatio(7,vh );
		@include center();
		justify-content: space-between;
		flex-direction: column;
		border-radius: 1.5vh;
		padding: 2vh;
		img{
			width: 75%;
			height: auto;
		}
		h1{
			grid-area: ty;
			font-size: 300%;
			margin: 0%;
		}
		em{
			color: $greytext;
			font-size: 150%;
			margin: 0%;
		}
	}
</style>
<!-- Kod av Felix Lindblom aka Fennessy -->