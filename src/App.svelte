<script lang="ts">
  import { XsdPattern } from "xsd-pattern-js"

  export let pattern: string = ""
  export let testValue: string = ""
  $: result = checkPattern(pattern, testValue)

  const checkPattern = (pattern: string, testValue: string) => {
    const xsdPattern = new XsdPattern(pattern)

    return {
      isPatternValid: xsdPattern.isValid(),
      isInputValid: xsdPattern.isValid().result && xsdPattern.match(testValue)
    }
  }
</script>

<main>
  <h1>Xsd Pattern Tester</h1>
  <div class="input">
    <div>
      <label for="pattern">Pattern</label>
      <input
        id="pattern"
        bind:value={pattern}
        class={!result.isPatternValid.result ? "invalid" : "valid"}
        type="text"
      />
    </div>
    <div>
      <label for="testValue">Test value</label>
      <input
        id="testValue"
        bind:value={testValue}
        class={result.isPatternValid.result && !result.isInputValid
          ? "invalid"
          : "valid"}
        type="text"
      />
    </div>
  </div>
  {#if !result.isPatternValid.result}
    <p class="invalid">
      pattern {pattern} is not valid because {result.isPatternValid.reason}.
    </p>
  {:else}
    <p class={!result.isInputValid ? "invalid" : "valid"}>
      The input "{testValue}" is {result.isInputValid} against "{pattern}".
    </p>
  {/if}
</main>

<style>
  main {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  .input {
    display: flex;
  }

  .input > div {
    padding: 20px;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }

  input.invalid {
    outline: 2px solid red;
  }

  p {
    padding: 2rem 4rem;
    margin: 0;
  }

  p.invalid {
    background-color: rgba(247, 59, 59, 0.5);
  }

  p.valid {
    background-color: rgba(0, 128, 0, 0.5);
  }
</style>
