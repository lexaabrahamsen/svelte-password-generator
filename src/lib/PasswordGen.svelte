<script lang="ts">
  import { fade } from 'svelte/transition';

  let password = "Select a password length";
  let menuSelection = "8";
  $: passwordLength = Number(menuSelection);

  const charGroups = [
    { name: 'lowercase', min: 97, max: 123 },
    { name: 'uppercase', min: 65, max: 91 },
    { name: 'numbers', min: 48, max: 58 },
    { name: 'symbols', min: 33, max: 43 },
  ];

  const makeRandomCharacter = () => {
    let charType = Math.floor(Math.random() * charGroups.length);
    let low = charGroups[charType].min;
    let high = charGroups[charType].max;

    let number = Math.floor(Math.random() * (high - low)) + low;
    return number;
  };

  const makeRandomPassword = (codelength: number) => {
    let code = "";
    for(let i = 0; i < codelength; i++) {
      let char = String.fromCharCode(makeRandomCharacter());
      code += char;
    }
    password = code;
  }

  const copyPassword = () => {
    let pw = document.querySelector<HTMLInputElement>('#code');
    pw?.select();
    pw?.setSelectionRange(0, 99999);
    document.execCommand("copy");
  };

</script>

<section id="pw-cont" transition:fade={{ duration: 200 }}>
  <span class="close">&times;</span>
  <input type="text" id="code" readonly bind:value={password} />

  <select name="pw-length" id="pw-length-selector" bind:value={menuSelection}>
    <option value="8">8</option>
    <option value="12">12</option>
    <option value="16">16</option>
    <option value="20">20</option>
  </select>

  <div>
    <button id="code-btn" on:click={() => makeRandomPassword(passwordLength)}>make password</button>
    <button id="copy-btn" on:click={copyPassword}>copy</button>
  </div>
</section>

<style>
	#pw-cont {
		width: 90%;
		height: auto;
		position: absolute;
		top: 5%;
    left: 50%;
    transform: translate(-50%, 10%); /* center modal */
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: flex-start;
		background-color: #ddd;
		box-shadow: 0 0 15px 1px black;
	}

	input#code {
		width: 470px;
		height: 40px;
		margin-top: 20px;
		padding: 25px;
		font-size: 1.4rem;
		font-weight: bold;
		border: 1px solid black;
		text-align: center;
		overflow: auto;
	}

	button {
		width: 165px;
		padding: 10px;
		font-size: 1.3rem;
		background-color: #F2B705;
		color: black;
		cursor: pointer;
	}
	
	#copy-btn {
		background-color: #F24405;
		color: white;
	}

	#pw-length-selector {
		width: 340px;
		height: 45px;
		font-size: 1.3rem;
		margin: 0 0 20px 0;
		cursor: pointer;
	}
	
	/* The Close Button */
	.close {
		position: absolute;
		bottom: 10px;
		right: 15px;
		color: #000;
		font-size: 40px;
		font-weight: bold;
		transition: 0.3s;
	}

	.close:hover,
	.close:focus {
		color: #bbb;
		text-decoration: none;
		cursor: pointer;
	}

</style>