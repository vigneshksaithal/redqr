<script lang="ts">
  import QRCode from 'qrcode';
  import { onMount } from 'svelte';
  import { saveAs } from 'file-saver';

  let text = 'link',
    canvasEl: HTMLCanvasElement;

  onMount(() => {
    getQRCode();
  });

  async function getQRCode() {
    await QRCode.toCanvas(canvasEl, text);
  }

  async function downloadPng() {
    canvasEl.toBlob(function (blob: any) {
      saveAs(blob, 'qr-code.png');
    });
  }

  async function downloadSvg() {
    await QRCode.toString(text, {
      type: 'svg',
    }).then((svg) => {
      var blob = new Blob([svg]);
      saveAs(blob, 'qr-code.svg');
    });
  }
</script>

<main class="grid grid-cols-1 lg:grid-cols-2 gap-8 mx-auto max-w-4xl">
  <div class="bg-slate-200 p-10">
    <canvas class="mx-auto h-96 w-96 m-6" bind:this={canvasEl} />
    <div class="flex justify-center gap-6 pt-4">
      <button class="bg-slate-800 text-white" on:click={downloadPng}
        >Download PNG</button
      >
      <button class="bg-slate-800" on:click={downloadSvg}>Download SVG</button>
    </div>
  </div>
  <div class="justify-self-center">
    <h1 class="text-2xl font-bold">QR Code Generator</h1>
    <p class="text-gray-500 mb-4">
      Generate QR Code for any text or URL. You can download the QR Code as PNG
      or SVG.
    </p>
    <label for="QR Code text">Text/URL</label>
    <input class="" type="text" bind:value={text} on:input={getQRCode} />
  </div>
</main>

<style lang="postcss">
  canvas {
    height: 500px;
    width: 500px;
  }
</style>
