<script>
    export let name;
    let gameStarted = false; // Houdt bij of het spel gestart is
  
    // Spelerpositie en grootte van het speelveld
    let player = { x: 1, y: 1 }; // Beginpositie van de speler
    let gridSize = 30; // Vergroot de grootte van elk vakje in het speelveld
  
    // Doolhof met muren (0) en stippen (2)
    let maze = [
      [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      [0, 1, 2, 1, 1, 1, 1, 2, 1, 0],
      [0, 1, 0, 1, 0, 0, 1, 0, 1, 0],
      [0, 1, 0, 2, 1, 1, 1, 0, 1, 0],
      [0, 1, 1, 1, 0, 0, 1, 1, 1, 0],
      [0, 1, 0, 1, 1, 1, 1, 0, 2, 0],
      [0, 1, 1, 1, 0, 0, 1, 2, 1, 0],
      [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    ];
  
    let score = 0; // Houdt score bij
    let totalDots = countTotalDots(); // Telt het aantal stippen
    let won = false; // Houdt bij of de speler gewonnen heeft
  
    // Telt het totale aantal stippen in het doolhof
    function countTotalDots() {
      return maze.flat().filter(cell => cell === 2).length; // Telt alle stippen
    }
  
    // Controleert of de volgende positie een muur is
    const isWall = (x, y) => maze[y][x] === 0;
    // Controleert of de volgende positie een stip is
    const isDot = (x, y) => maze[y][x] === 2;
  
    // Controleert of de speler gewonnen heeft
    const checkWin = () => {
      if (score === totalDots) {
        won = true; // Set won naar true als alle stippen zijn verzameld
      }
    };
  
    // Beweegt de speler op basis van de richting
    const movePlayer = (direction) => {
      if (won) return; // Stop als het spel gewonnen is
  
      let newX = player.x; // Nieuwe x-coördinaat
      let newY = player.y; // Nieuwe y-coördinaat
  
      // Bepaal de nieuwe positie op basis van de richting
      if (direction === 'up') newY = Math.max(0, player.y - 1);
      if (direction === 'down') newY = Math.min(maze.length - 1, player.y + 1);
      if (direction === 'left') newX = Math.max(0, player.x - 1);
      if (direction === 'right') newX = Math.min(maze[0].length - 1, player.x + 1);
  
      // Controleer of de nieuwe positie geen muur is
      if (!isWall(newX, newY)) {
        player.x = newX; // Update de speler x-coördinaat
        player.y = newY; // Update de speler y-coördinaat
  
        // Als er een stip is, verzamel deze
        if (isDot(newX, newY)) {
          score += 1; // Verhoog de score
          maze[newY][newX] = 1; // Verander stip naar een muur
          checkWin(); // Controleer of er gewonnen is
        }
      }
    };
  
    // Start het spel
    const startGame = () => gameStarted = true;
    // Ga terug naar het startscherm
    const goBackToStart = () => {
      gameStarted = false; // Reset het spel
      score = 0; // Reset score
      player = { x: 1, y: 1 }; // Reset speler positie
      maze = [
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 1, 2, 1, 1, 1, 1, 2, 1, 0],
        [0, 1, 0, 1, 0, 0, 1, 0, 1, 0],
        [0, 1, 0, 2, 1, 1, 1, 0, 1, 0],
        [0, 1, 1, 1, 0, 0, 1, 1, 1, 0],
        [0, 1, 0, 1, 1, 1, 1, 0, 2, 0],
        [0, 1, 1, 1, 0, 0, 1, 2, 1, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      ]; // Reset het doolhof
    };
  </script>
  
  <div class="gameboy">
    <div class="screen">
      {#if !gameStarted}
        <div class="start-screen">
          <h1>Welkom, {name}!</h1>
          <div class="button-container">
            <button on:click={startGame}>Speel Spel</button> <!-- Start het spel -->
            <button on:click={() => window.location.href = '/'}>Stoppen</button> <!-- Stop het spel -->
          </div>
        </div>
      {:else}
        {#if won}
          <div class="win-screen">
            <h2>Gefeliciteerd, {name}!</h2>
            <p>Je hebt alle stippen verzameld!</p>
            <button on:click={goBackToStart}>Terug naar Start</button> <!-- Terug naar het startscherm -->
          </div>
        {:else}
          <div class="game-screen">
            <svg viewBox="0 0 {maze[0].length * gridSize} {maze.length * gridSize}">
              {#each maze as row, y}
                {#each row as cell, x}
                  <rect x={x * gridSize} y={y * gridSize} width={gridSize} height={gridSize} fill={cell === 0 ? 'black' : 'white'} />
                  {#if cell === 2}
                    <circle cx={x * gridSize + gridSize / 2} cy={y * gridSize + gridSize / 2} r={gridSize / 6} fill="yellow" /> <!-- Teken stippen -->
                  {/if}
                {/each}
              {/each}
              <circle cx={player.x * gridSize + gridSize / 2} cy={player.y * gridSize + gridSize / 2} r={gridSize / 2.5} fill="blue" /> <!-- Teken de speler -->
            </svg>
            <p>Score: {score}/{totalDots}</p> <!-- Toon de score -->
          </div>
        {/if}
      {/if}
    </div>
  
    <div class="controls">
      <div class="left-controls">
        <button on:click={() => movePlayer('up')}>↑</button> <!-- Beweeg omhoog -->
        <div class="horizontal-buttons">
          <button on:click={() => movePlayer('left')}>←</button> <!-- Beweeg naar links -->
          <button on:click={() => movePlayer('right')}>→</button> <!-- Beweeg naar rechts -->
        </div>
        <button on:click={() => movePlayer('down')}>↓</button> <!-- Beweeg omlaag -->
        <button on:click={() => movePlayer('down')}>Start</button> <!-- Start button om te bevestigen -->
      </div>
    </div>
  </div>
  
  <style>
    .gameboy {
      width: 300px;
      height: 400px; /* Verhoogde hoogte voor meer ruimte */
      background-color: #c4c4c4; /* Grijze achtergrondkleur */
      border-radius: 10px; /* Afgeronde hoeken */
      padding: 20px; /* Ruimte binnenin */
      box-shadow: 5px 5px 20px rgba(0, 0, 0, 0.2); /* Schaduw voor diepte */
      position: relative; /* Positionering voor knoppen */
      display: flex;
      flex-direction: column; /* Stapelen van elementen */
    }
  
    .screen {
      width: 100%;
      height: 250px; /* Hoogte van het scherm verhoogd voor meer speelruimte */
      background-color: #99c2a2; /* Groene achtergrondkleur */
      border: 4px solid #000; /* Zwarte rand */
      border-radius: 5px; /* Afgeronde hoeken van het scherm */
      display: flex;
      align-items: center; /* Centreer de inhoud verticaal */
      justify-content: center; /* Centreer de inhoud horizontaal */
    }
  
    .start-screen {
      text-align: center; /* Centreer de tekst */
    }
  
    .button-container {
      display: flex;
      justify-content: center; /* Centreer de knoppen */
      gap: 10px; /* Ruimte tussen de knoppen */
      margin-top: 10px; /* Ruimte boven de knoppen */
    }
  
    button {
      padding: 10px 15px; /* Ruimte binnen de knoppen */
      font-size: 16px; /* Grootte van de tekst in de knoppen */
      cursor: pointer; /* Cursor verandert in pointer */
    }
  
    .controls {
      display: flex; /* Gebruik flexbox voor de knoppen */
      justify-content: center; /* Centreer de knoppen horizontaal */
      margin-top: 10px; /* Ruimte boven de knoppen */
    }
  
    .left-controls {
      display: flex; /* Gebruik flexbox voor de verticale knoppen */
      flex-direction: column; /* Stapel de knoppen verticaal */
      align-items: center; /* Centreer de knoppen horizontaal */
    }
  
    .horizontal-buttons {
      display: flex; /* Gebruik flexbox voor de horizontale knoppen */
      gap: 10px; /* Ruimte tussen de knoppen */
    }
  </style>
  