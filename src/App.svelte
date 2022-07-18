<script>
  import SingleCard from './lib/SingleCard.svelte';

  import imgCover from './assets/0.jpg'

  import album1 from './assets/1.jpg'
  import album2 from './assets/2.jpg'
  import album3 from './assets/3.jpg'
  import album4 from './assets/4.jpg'
  import album5 from './assets/5.jpg'
  import album6 from './assets/6.jpg'
  import album7 from './assets/7.jpg'
  import album8 from './assets/8.jpg'
  import album9 from './assets/9.jpg'

  const cardImages = [
    {src: album1, matched: false},
    {src: album2, matched: false},
    {src: album3, matched: false},
    {src: album4, matched: false},
    {src: album5, matched: false},
    {src: album6, matched: false},
    {src: album7, matched: false},
    {src: album8, matched: false},
    {src: album9, matched: false},
  ]

  let cards = []
  let turns = 0
  let choiceOne = null
  let choiceTwo = null
  let disabled = false

  const shuffledCards = () => {
    const shuffledCards = [...cardImages, ...cardImages]
    .sort(() => Math.random() - 0.5)
    .map((card) => ({...card, id:Math.random()}))
    cards = shuffledCards
    turns = 0
  }

  const handleChoice = card => {
    choiceOne ? choiceTwo = card : choiceOne = card
  }

  $: if (choiceOne && choiceTwo) {
    disabled = true
    if (choiceOne.src === choiceTwo.src) {
      cards = cards.map(card => {
        if (card.src === choiceOne.src) {
          return {...card, matched:true}
        } else {
          return card
        }
      })
      resetTurn()
    } else {
      setTimeout(() => resetTurn(), 1000)
    }
  }

  const resetTurn = () => {
    choiceOne = null
    choiceTwo = null
    turns = turns + 1
    disabled = false
  }

</script>

<main>
    <div class="App">
      <h1>Memory game Radiohead</h1>
      <button on:click={shuffledCards}>New Game</button>
      <p>Turns: {turns}</p>
      <div class="cards-grid">
        {#each cards as card (card.id)}
		      <SingleCard 
		      	{card}
		      	{imgCover} 
		      	{disabled}
		      	{handleChoice}
		      	flipped={
		      		card === choiceOne || 
		      		card === choiceTwo || 
		      		card.matched} />
	      {/each}
      </div>
    </div>
</main>

<style>
.App {
  margin: 0;
  min-height: 100vh;
  text-align: center;
}
button {
  background: #1e1e1e;
  padding: 1rem;
  color: white;
  border-radius: 0 0 15px 0;
  cursor: pointer;
  font-size: 1em;
}
.cards-grid {
	margin-top: 4px;
	display: grid;
	grid-template-columns: repeat(6, 1fr);
	grid-gap: 2px;
}
</style>