<script>
    export let name;
    export let info;
  
    let screenMessage = "Press Start!";
    
    // Simulate Start button
    const handleStart = () => {
      screenMessage = `Hello, I'm ${name}!`;
    };
  </script>
  
  <div class="gameboy">
    <div class="screen">
      <div class="screen-content">
        <p>{screenMessage}</p>
        <p>{info}</p>
      </div>
    </div>
    <div class="controls">
      <button aria-label="Start button" on:click={handleStart}>Start</button>
      <button aria-label="Select button">Select</button>
    </div>
  </div>
  
  <style>
    .gameboy {
      width: 300px;
      background-color: #c4c4c4;
      border-radius: 10px;
      padding: 20px;
      box-shadow: 5px 5px 20px rgba(0, 0, 0, 0.2);
      position: relative;
    }
  
    /* Screen styles */
    .screen {
      width: 100%;
      height: 150px;
      background-color: #99c2a2;
      border: 4px solid #000;
      border-radius: 5px;
      display: flex;
      justify-content: center;
      align-items: center;
      margin-bottom: 20px;
      position: relative;
    }
  
    .screen-content {
      font-family: 'Courier New', monospace;
      color: #333;
      text-align: center;
      font-size: 14px;
    }
  
    /* Controls styles */
    .controls {
      display: flex;
      justify-content: space-between;
      margin-top: 10px;
    }
  
    button {
      background-color: #444;
      color: #fff;
      border: none;
      padding: 10px 15px;
      border-radius: 5px;
      cursor: pointer;
      font-family: inherit;
    }
  
    button:active {
      background-color: #666;
    }
  
    @media (max-width: 400px) {
      .gameboy {
        width: 100%;
      }
    }
  </style>
  