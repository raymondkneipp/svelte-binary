<script lang="ts">
  
  export let value;
  let indicatorPosition = "0rem";

  $: {
    switch (value) {
      case "dark":
        indicatorPosition = "0rem";
        break;
      case "green":
        indicatorPosition = "4rem";
        break;
      case "red":
        indicatorPosition = "8rem";
        break;
      case "dracula":
        indicatorPosition = "12rem";
        break;
      case "purple":
        indicatorPosition = "16rem";
        break;
      case "contrast":
        indicatorPosition = "20rem";
        break;
      case "yellow":
        indicatorPosition = "24rem";
        break;
    }
  }

</script>
  
<div class="operators">
  <label>
    <input type="radio" name="theme" value="dark" bind:group={value} />
    <div class="option">
      dark
    </div>
  </label>

  <label>
    <input type="radio" name="theme" value="green" bind:group={value} />
    <div class="option">
      green
    </div>
  </label>
    
  <label>
    <input type="radio" name="theme" value="red" bind:group={value} />
    <div class="option">
      red
    </div>
  </label>
    
  <label>
    <input type="radio" name="theme" value="dracula" bind:group={value} />
    <div class="option">
      blue
    </div>
  </label>
    
  <label>
    <input type="radio" name="theme" value="purple" bind:group={value} />
    <div class="option">
      purple
    </div>
  </label>
    
  <label>
    <input type="radio" name="theme" value="contrast" bind:group={value} />
    <div class="option">
      gray
    </div>
  </label>
    
  <label>
    <input type="radio" name="theme" value="yellow" bind:group={value} />
    <div class="option">
      yellow
    </div>
  </label>

  <div class="indicator" style={`left: ${indicatorPosition};`}></div>
</div>

<style>

  .operators {
    display: flex;
    position: relative;
    justify-content: center;
    width: 28rem;
    height: 2rem;
    border-radius: 0.5rem;
    margin: auto;
    background-color: var(--dark);
  }
  
  .indicator {
    background-color: var(--primary);
    width: 4rem;
    height: 2rem;
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
    width: 4rem;
    height: 2rem;
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
