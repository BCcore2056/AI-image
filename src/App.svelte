<script lang="ts">
  import { HfInference } from "@huggingface/inference";
  userInput: string;
  let imagePromise: Promise<string>;

  const hf = new HfInference(import.meta.env.VITE_HUGGING_FACE_ACCESS_TOKEN);

  async function ask() {
    const blob = await hf.textToImage({
      model: "stabilityai/stable-diffusion-xl-base-1.0",
      inputs: userInput,
    });
    return URL.createObjectURL(blob);
  }

  function generateImage() {
    imagePromise = ask();
  }
</script>

<main>
    <input bind:value={userInput} />
    <button on:click={generateImage}>Generate</button>
    {#if imagePromise}
      {#await imagePromise}
        Loading...
      {:then result}
        <img width=320 height=320 src={result} alt="Ai generated image"/>
      {/await}
    {:else}
      No image generated
    {/if}
</main>