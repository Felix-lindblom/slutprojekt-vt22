<script>
	
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
				<figure>
					<img src="../img/fennyJack.png" alt="Fenny jack logo">
					<h1>J</h1>
					<p><em>10</em></p>
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

	$headercol: rgb(230, 139, 20) ;
	$vlcol:rgb(100, 25, 5) ;
	$Whittext:rgb(250,250,250);
	$cardback:rgb(255, 250, 235);
	$pokergreen:rgb(53,101,77);
	$greybak:rgb(75, 75, 75);
	
	header{
		height: 10vh;
		background-color: $headercol;
		display: flex;
		justify-content: space-between;
		img{
			@include sidesPad(1vw);
		}
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

		.houseArea{
			@include center();
			
			figure{
				background-color: $cardback;
				height: 30rem;
				width: 20rem;
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
		}
	}
</style>