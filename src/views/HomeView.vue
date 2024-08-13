<template>
    <div class="home">
      <header class="home-header">
        <h1>Benvenuto a Battaglia Navale</h1>
        <p>Un gioco classico di strategia e fortuna!</p>
      </header>
  
      <section class="game-introduction">
        <h2>Come si gioca</h2>
        <p>
          Posiziona le tue navi sulla griglia e cerca di affondare le navi dell'avversario. 
          Ogni giocatore alterna i turni per sparare e tentare di colpire le navi nemiche.
          Il primo giocatore che affonda tutte le navi dell'avversario vince!
        </p>
      </section>
  
      <button class="start-game-btn" @click="startGame">Inizia il Gioco</button>
    </div>
  </template>
  
  <script>
  export default {
    name: 'HomeView',
    methods: {
      startGame() {
        this.$router.push('/game'); // Modifica il percorso in base alla tua configurazione delle rotte
      }
    }
  }
  </script>
  
  <style scoped>
  .home {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
    padding: 20px;
    text-align: center;
  }
  
  .home-header {
    margin-bottom: 20px;
  }
  
  .home-header h1 {
    font-size: 2.5rem;
    margin: 0;
  }
  
  .home-header p {
    font-size: 1.2rem;
    color: #555;
  }
  
  .game-introduction {
    margin-bottom: 20px;
  }
  
  .game-introduction h2 {
    font-size: 2rem;
    margin-bottom: 10px;
  }
  
  .game-introduction p {
    font-size: 1.2rem;
    line-height: 1.5;
    max-width: 600px;
    margin: 0 auto;
  }
  
  .start-game-btn {
    background-color: #4CAF50;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 5px;
    font-size: 1.2rem;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  .start-game-btn:hover {
    background-color: #3e8e41;
  }
  </style>
  