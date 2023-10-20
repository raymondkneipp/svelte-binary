<script lang="ts">
  import Byte from "../components/Byte.svelte"
  import Theme from "../components/Theme.svelte"

  let bytes: Uint8Array = new Uint8Array([0, 255]);
  
  let theme: "dark" | "green" | "red" | "dracula" | "purple" | "contrast" | "yellow" = "dark";

  let operator: "and" | "or" | "xor" | "nand" | "nor" = "and";
  let indicatorPosition = "0rem";

  $: result = bytes[0] & bytes[1];

  $: {
    switch (operator) {
      case "and":
        result = bytes[0] & bytes[1];
        indicatorPosition = "0rem";
        break;
      case "or":
        result = bytes[0] | bytes[1];
        indicatorPosition = "3rem";
        break;
      case "xor":
        result = bytes[0] ^ bytes[1];
        indicatorPosition = "6rem";
        break;
      case "nand":
        result = 255 - (bytes[0] & bytes[1]);
        indicatorPosition = "9rem";
        break;
      case "nor":
        result = 255 - (bytes[0] | bytes[1]);
        indicatorPosition = "12rem";
        break;
    }
  }

</script>
  
<div class={`page ${theme}`}>
  <div class="wrapper">
    <Theme bind:value={theme} />
    
    {#each bytes as byte}
      <Byte bind:value={byte} />
    {/each}
      
    <div class="operators">
      <label>
        <input type="radio" name="operator" value="and" bind:group={operator} />
        <div class="option">
          and
        </div>
      </label>

      <label>
        <input type="radio" name="operator" value="or" bind:group={operator} />
        <div class="option">
          or
        </div>
      </label>
        
      <label>
        <input type="radio" name="operator" value="xor" bind:group={operator} />
        <div class="option">
          xor
        </div>
      </label>
        
      <label>
        <input type="radio" name="operator" value="nand" bind:group={operator} />
        <div class="option">
          nand
        </div>
      </label>
        
      <label>
        <input type="radio" name="operator" value="nor" bind:group={operator} />
        <div class="option">
          nor
        </div>
      </label>

      <div class="indicator" style={`left: ${indicatorPosition};`}></div>
    </div>

    <Byte disabled={true} value={result} />
  </div>
</div>

<style>
  :global(body) {
    font-family: monospace;
    margin: 0;
    transition: * 0.3s;
  }

  .green {
    --primary: #4A7856;
    --primary-light: #94ECBE;
  
    --base: #1A1F16;
    --darker: #1E3F20;
    --dark: #345830;
    --darkish: #4A7856;
  }
  
  .red {
    --primary: #FF5555;        /* A vibrant red */
    --primary-light: #FFA9A9;  /* A lighter shade of red */

    --base: #1F161A;
    --darker: #3F201E;
    --dark: #583034;
    --darkish: #78564A;
  }
  
  .dracula {
    --primary: #6272A4;
    --primary-light: #B6B9CA;

    --base: #282A36;
    --darker: #1B1E2A;
    --dark: #44475A;
    --darkish: #565C72;

    --fg: #F8F8F2;      /* Bright text color */
    --muted: #6272A4;   /* Dimmer text color */
  }
  
  .purple {
    --primary: #A47EB1;
    --primary-light: #D2C4E6;

    --base: #2E273A;
    --darker: #1E1829;
    --dark: #4F4576;
    --darkish: #6F5E97;

    --fg: #F0E7E9;      /* Bright text color */
    --muted: #A47EB1;   /* Dimmer text color */
  }
  
  .contrast {
    --primary: #444444;        /* Black background */
    --primary-light: #FFFFFF;  /* White text */

    --base: #333333;          /* Slightly darker background */
    --darker: #000000;        /* Very dark background */
    --dark: #666666;          /* Dark text */
    --darkish: #999999;       /* Slightly lighter text */

    --fg: #FFFFFF;            /* Bright text color */
    --muted: #EEEEEE;         /* Dimmer text color */
  }
  
  .yellow {
    --primary: #FFD700;      /* Yellow background */
    --primary-light: #FFFFE0;  /* Light yellow text */

    --base: #FFC400;         /* Slightly darker background */
    --darker: #FFB200;       /* Darker background */
    --dark: #FFA000;         /* Dark text */
    --darkish: #FF8800;      /* Slightly lighter text */

    --fg: #000000;           /* Bright text color */
    --muted: #333333;        /* Dimmer text color */
  }
  
  :root {
    --primary: #3333AA;
    --primary-light: #CCCCFF;
  
    --base: #121212;
    --darker: #191919;
    --dark: #222222;
    --darkish: #333333;

    --fg: #ffffff;
    --muted: #a3a3a3;
  }

  .page {
    min-height: 100vh;
    width: 100vw;
    background-color: var(--base);
    padding: 1rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .wrapper {
    width: min-content;
    margin: auto;
    display: flex;
    flex-direction: column;
    gap: 2rem;
    align-items: center;
  }

  .operators {
    display: flex;
    position: relative;
    justify-content: center;
    width: 15rem;
    height: 3rem;
    border-radius: 0.5rem;
    margin: auto;
    background-color: var(--dark);
  }
  
  .indicator {
    background-color: var(--primary);
    width: 3rem;
    height: 3rem;
    border-radius: 0.5rem;
    position: absolute;
    z-index: 1;
    transition: left 0.3s;
  }

  input {
    position: absolute;
    width: 0;
    height: 0;
  }

  input:focus-visible ~ .option {
    outline: 3px solid var(--primary-light);
    outline-offset: -1px;
  }
  
  .option {
    border: none;
    padding: 0;
    margin: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 3rem;
    height: 3rem;
    overflow: hidden;
    color: var(--fg);
    cursor: pointer;
    transition: outline 0.2s ease-out, background-color 0.2s ease-out;
    user-select: none;
    box-sizing: border-box;
    position: relative;
    z-index: 10;
    border-radius: 0.5rem;
  }
  
</style>
