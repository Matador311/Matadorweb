<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Matador Hafıza Kartları Oyunu</title>
<style>
  body {
    background: linear-gradient(45deg, #1e1e2f, #4b6cb7);
    color: white;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
    min-height: 100vh;
  }
  h1 {
    margin-bottom: 20px;
    text-shadow: 0 0 10px #000;
  }
  #game {
    display: grid;
    grid-template-columns: repeat(4, 80px);
    grid-gap: 15px;
  }
  .card {
    width: 80px;
    height: 80px;
    background: #2c3e50;
    border-radius: 12px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 40px;
    cursor: pointer;
    user-select: none;
    box-shadow: 0 4px 8px rgba(0,0,0,0.5);
    transition: transform 0.3s;
  }
  .card.flipped, .card.matched {
    background: #1abc9c;
    color: white;
    cursor: default;
    transform: rotateY(180deg);
  }
  .card.matched {
    background: #27ae60;
    box-shadow: 0 0 20px #27ae60;
  }
</style>
</head>
<body>

<h1>Matador Hafıza Kartları</h1>
<div id="game"></div>

<script>
  const cardsArray = ['🍎','🍌','🍇','🍉','🍓','🍒','🥝','🍍']; // 8 çift

  let cards = [...cardsArray, ...cardsArray]; // çiftler
  let gameContainer = document.getElementById('game');
  let flippedCards = [];
  let matchedCount = 0;
  let lockBoard = false;

  // Karıştır
  function shuffle(array) {
    for(let i = array.length -1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [array[i], array[j]] = [array[j], array[i]];
    }
    return array;
  }

  function createBoard() {
    shuffle(cards);
    cards.forEach((symbol, index) => {
      const card = document.createElement('div');
      card.classList.add('card');
      card.dataset.symbol = symbol;
      card.dataset.index = index;
      card.innerText = '';
      card.addEventListener('click', flipCard);
      gameContainer.appendChild(card);
    });
  }

  function flipCard() {
    if(lockBoard) return;
    if(this.classList.contains('flipped') || this.classList.contains('matched')) return;

    this.classList.add('flipped');
    this.innerText = this.dataset.symbol;

    flippedCards.push(this);

    if(flippedCards.length === 2) {
      lockBoard = true;
      setTimeout(checkForMatch, 1000);
    }
  }

  function checkForMatch() {
    const [card1, card2] = flippedCards;
    if(card1.dataset.symbol === card2.dataset.symbol) {
      card1.classList.add('matched');
      card2.classList.add('matched');
      matchedCount += 2;
      if(matchedCount === cards.length) {
        setTimeout(() => alert('Matador tebrikler! Oyunu kazandın! 🎉'), 300);
      }
    } else {
      card1.classList.remove('flipped');
      card2.classList.remove('flipped');
      card1.innerText = '';
      card2.innerText = '';
    }
    flippedCards = [];
    lockBoard = false;
  }

  createBoard();
</script>

</body>
</html>
