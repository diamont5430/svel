<script lang="ts">
  import { HfInference } from "@huggingface/inference"; // importujemy potrzebne narzędzie z biblioteki HuggingFace
  let userInput: string;
  let imagePromise: Promise<string>;
  const hf = new HfInference(import.meta.env.VITE_HUGGING_FACE_ACCESS_TOKEN); // konfigurujemy narzędzie z naszym kluczem dostępu

  async function ask() {
    const blob = await hf.textToImage({
      // interesujący nas sposób interakcji z danym modelem - w tym wypadku wpisany tekst zamieniany na wygenerowany obraz
      model: "stabilityai/stable-diffusion-xl-base-1.0", // konkretny model ze strony HuggingFace
      inputs: userInput, // dane wejściowe użytkownika
    });
    return URL.createObjectURL(blob);
  }
  function generateImage(){
    imagePromise = ask();
  }
</script>

<main>
    <!-- Tutaj wpisz własny kod układu strony -->
    <input bind:value={userInput} />
    <button on:click={generateImage}>Generate</button>
    {#await imagePromise}
      Loading...
    {:then result}
      <img src={result}/>
    {:catch}
      OPPS! Failed to generate the image
    {/await}
</main>

<style>
    /* Tu znajdzie się kod odpowiadający za wygląd strony */
    main{
      background-color: rgb(64, 79, 216);
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      width:100%;
      height: 100%;
    }

    input{
      padding: 4px;
      border: solid;
      border-radius: 10px;
      text-align: center;
    }
</style>
