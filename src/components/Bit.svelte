<script lang="ts">
  import { draw } from 'svelte/transition';

  export let value: boolean = false;
  export let toggle;
  export let bitPosition: number;
  export let disabled: boolean = false;

</script>

<label>
  <span>{2**bitPosition}</span>
  <input type="checkbox" checked={value} on:change={toggle(bitPosition)} disabled={disabled} />

  <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-power-off">
    <path d="M18.36 6.64a9 9 0 1 1-12.73 0" />
    <line x1="12" x2="12" y1="2" y2="12" />
  </svg>

  <div class="toggle"></div>
</label>

<style>

  label {
    height: 6rem;
    width: 3rem;
    background-color: var(--darker);
    border-radius: 0.5rem;
    position: relative;
    transition: outline 0.3s, background-color 0.3s;
    
    &:has(> input:enabled) {
      cursor: pointer;
      
      &:hover {
        background-color: var(--darkish);
      }
    }
  
    &:has(> input:checked) span {
      color: var(--fg);
    }
  
    &:has(> input:focus-visible) {
      outline: 3px solid var(--primary-light);
    }

    & input {
      width: 0;
      height: 0;
      position: absolute;
    }

    & span {
      color: var(--muted);
      height: 3rem;
      width: 3rem;
      display: flex;
      align-items: center;
      justify-content: center;
      position: relative;
      z-index: 10;
      transition: color 0.3s;
    }
    
    & svg {
      height: 3rem;
      width: 3rem;
      padding: 1rem;
      box-sizing: border-box;
      position: relative;
      z-index: 10;
      color: var(--muted);
    }
    
    & .toggle {
      width: 3rem;
      height: 3rem;
      position: absolute;
      background-color: var(--dark);
      top: 50%;
      z-index: 1;
      transition: top 0.3s, background-color 0.3s;
      border-radius: 0.5rem;
    }
    
    &:has(> input:checked) .toggle {
      background-color: var(--primary);
      top: 0;
    }
  }
  
</style>
