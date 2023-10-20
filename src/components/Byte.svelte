<script lang="ts">
  import Bit from "../components/Bit.svelte"
  import { fly } from 'svelte/transition';

  export let value;
  export let disabled: boolean = false;

  $: bool_arr = value.toString(2).padStart(8, '0').split('').map(str => str === '1' ? true : false);

  $: binary = value.toString(2).padStart(8, '0');
  $: hex = value.toString(16).toUpperCase();
  $: octal = value.toString(8);
  $: ascii = displayNonPrintableChar(value);
  
function displayNonPrintableChar(byteValue) {
  if (byteValue <= 32) {
    // Handle non-printable characters
    switch (byteValue) {
      case 0:
        return "Null"; // [NUL]
      case 1:
        return "Start of Header"; // [SOH]
      case 2:
        return "Start of Text"; // [STX]
      case 3:
        return "End of Text"; // [ETX]
      case 4:
        return "End of Transmission"; // [EOT]
      case 5:
        return "Enquiry"; // [ENQ]
      case 6:
        return "Acknowledge"; // [ACK]
      case 7:
        return "Bell (beep)"; // [BEL]
      case 8:
        return "Backspace"; // [BS]
      case 9:
        return "Horizontal Tab"; // [HT]
      case 10:
        return "Line Feed"; // [LF]
      case 11:
        return "Vertical Tab"; // [VT]
      case 12:
        return "Form Feed"; // [FF]
      case 13:
        return "Carriage Return"; // [CR]
      case 14:
        return "Shift Out"; // [SO]
      case 15:
        return "Shift In"; // [SI]
      case 16:
        return "Data Link Escape"; // [DLE]
      case 17:
        return "Device Control 1"; // [DC1]
      case 18:
        return "Device Control 2"; // [DC2]
      case 19:
        return "Device Control 3"; // [DC3]
      case 20:
        return "Device Control 4"; // [DC4]
      case 21:
        return "Negative Acknowledge"; // [NAK]
      case 22:
        return "Synchronous Idle"; // [SYN]
      case 23:
        return "End of Transmission Block"; // [ETB]
      case 24:
        return "Cancel"; // [CAN]
      case 25:
        return "End of Medium"; // [EM]
      case 26:
        return "Substitute"; // [SUB]
      case 27:
        return "Escape"; // [ESC]
      case 28:
        return "File Separator"; // [FS]
      case 29:
        return "Group Separator"; // [GS]
      case 30:
        return "Record Separator"; // [RS]
      case 31:
        return "Unit Separator"; // [US]
      case 32:
        return "Space";
    }
  } else {
    // Display printable characters as is
    return String.fromCharCode(byteValue);
  }
}
  
  function decimalToBase64(decimal) {
    const decimalString = decimal.toString();

    // Encode the string as Base64
    const base64Encoded = btoa(decimalString);

    return base64Encoded;
  }
  
  $: base64 = decimalToBase64(value);
  
  function toggle(bitPosition) {
    value ^= (1 << bitPosition);
  }

  function invert() {
    value = ~value & 0xff;
  }

  function shiftLeft() {
    value = (value << 1) & 0xff;
  }
  
  function shiftRight() {
    value = value >>> 1;
  }

</script>
  
<div class="wrapper">
  <div class="byte">
    {#each bool_arr as bool, i}
      <Bit value={bool} toggle={toggle} bitPosition={7-i} disabled={disabled} />
    {/each}
  </div>

  <div class="footer">
    <dl>
      <dt>Decimal</dt>
      <dd class="fly">
        {#key value}
          <div in:fly={{y: 10}} out:fly={{y: -10}}><strong>{value}</strong></div>
        {/key}
      </dd>
      <dt>Binary</dt>
      <dd class="fly">
        {#key binary}
          <div in:fly={{y: 10}} out:fly={{y: -10}}>0b<strong>{binary}</strong></div>
        {/key}
      </dd>
      <dt>Base64</dt>
      <dd class="fly">
        {#key base64}
          <div in:fly={{y: 10}} out:fly={{y: -10}}><strong>{base64}</strong></div>
        {/key}
      </dd>
      <dt>Octal</dt>
      <dd class="fly">
        {#key octal}
          <div in:fly={{y: 10}} out:fly={{y: -10}}>0o<strong>{octal}</strong></div>
        {/key}
      </dd>
      <dt>ASCII</dt>
      <dd class="fly">
        {#key ascii}
          <div in:fly={{y: 10}} out:fly={{y: -10}}><strong>{ascii}</strong></div>
        {/key}
      </dd>
      <dt>Hex</dt>
      <dd class="fly">
        {#key hex}
          <div in:fly={{y: 10}} out:fly={{y: -10}}>0x<strong>{hex}</strong></div>
        {/key}
      </dd>
    </dl>

    {#if disabled !== true}
      <div class="buttons">
        <button on:click={invert} title="Invert" aria-label="Invert">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-arrow-up-down"><path d="m21 16-4 4-4-4"/><path d="M17 20V4"/><path d="m3 8 4-4 4 4"/><path d="M7 4v16"/></svg>
        </button>
        <button on:click={shiftLeft} title="Shift Left" aria-label="Shift Left">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-chevrons-left"><path d="m11 17-5-5 5-5"/><path d="m18 17-5-5 5-5"/></svg>
        </button>
        <button on:click={shiftRight} title="Shift Right" aria-label="Shift Right">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-chevrons-right"><path d="m6 17 5-5-5-5"/><path d="m13 17 5-5-5-5"/></svg>
        </button>
      </div>
    {/if}
  </div>
</div>

<style>
  .fly {
    position: relative;
    
    & div {
      position: absolute;
    }
  }
  
  .wrapper {
    display: flex;
    align-items: stretch;
    gap: 1rem;
    flex-direction: column;
    width: min-content;
  }
  
  .byte {
    display: grid;
    grid-template-columns: repeat(8, 1fr);
    width: min-content;
    gap: 0.5rem;
  }

  dl {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    margin: 0;
    column-gap: 0.5rem;
    
    & dt {
      color: var(--muted);
      text-align: right;
    }
    
    & dd {
      margin: 0;
      white-space: nowrap;
      color: var(--muted);

      & strong {
        font-weight: bold;
        color: var(--fg);
      }
    }
  }

  p {
    color: var(--fg);
  }

  .footer {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
  }

  .buttons {
    display: flex;
    gap: 0.5rem;
    flex-direction: row;
    margin-left: 1rem;
  }

  button {
    border: none;
    padding: 1rem;
    margin: 0;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 3rem;
    height: 3rem;
    padding: 1rem;
    overflow: hidden;
    background-color: var(--dark);
    border-radius: 0.5rem;
    color: var(--fg);
    cursor: pointer;
    transition: outline 0.2s ease-out, background-color 0.2s ease-out;
    user-select: none;
    
    &:hover {
      background-color: var(--darkish);
    }
  
    &:focus-visible {
      outline: 3px solid var(--primary-light);
      outline-offset: -1px;
    }
  }
</style>
