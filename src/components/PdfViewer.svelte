<script>
  import { onMount } from 'svelte';
  import Icon from "@iconify/svelte";

  export let src;

  let canvas;
  let pdfDoc = null;
  let pageNum = 1;
  let pageRendering = false;
  let pageNumPending = null;
  let numPages = 0;
  let loading = true; // Add loading state

  async function renderPage(num) {
    pageRendering = true;
    const page = await pdfDoc.getPage(num);
    const viewport = page.getViewport({ scale: 1.5 });

    const context = canvas.getContext('2d');
    canvas.height = viewport.height;
    canvas.width = viewport.width;

    const renderContext = {
      canvasContext: context,
      viewport: viewport,
    };
    const renderTask = page.render(renderContext);

    renderTask.promise.then(() => {
      pageRendering = false;
      if (pageNumPending !== null) {
        renderPage(pageNumPending);
        pageNumPending = null;
      }
    });
  }

  function queueRenderPage(num) {
    if (pageRendering) {
      pageNumPending = num;
    } else {
      renderPage(num);
    }
  }

  function onPrevPage() {
    if (pageNum <= 1) {
      return;
    }
    pageNum--;
    queueRenderPage(pageNum);
  }

  function onNextPage() {
    if (pageNum >= numPages) {
      return;
    }
    pageNum++;
    queueRenderPage(pageNum);
  }

  onMount(async () => {
    const loadScript = (url) => {
      return new Promise((resolve, reject) => {
        const script = document.createElement('script');
        script.src = url;
        script.onload = resolve;
        script.onerror = reject;
        document.head.appendChild(script);
      });
    };

    try {
      await Promise.all([
        loadScript('https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js'),
        loadScript('https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js'),
      ]);

      const { pdfjsLib } = globalThis;
      pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js';

      pdfDoc = await pdfjsLib.getDocument(src).promise;
      numPages = pdfDoc.numPages;
      await renderPage(pageNum);
      loading = false; // Set loading to false after rendering
    } catch (error) {
      console.error('Error loading PDF:', error);
      loading = false; // Set loading to false even on error
    }
  });
</script>

<div class="relative">
  <canvas bind:this={canvas} class="rounded-md overflow-hidden" 
    style="width: 100%; aspect-ratio: 1/1.414;border: 1px solid var(--primary);"></canvas>

  {#if loading}
    <div class="flex items-center justify-center w-full h-full absolute top-0 left-0 bg-opacity-75 z-10">
      <Icon icon="line-md:downloading-loop" class="text-[2.5rem]"></Icon>
    </div>
  {:else}
    <a
      href={src}
      download
      class="btn-regular rounded-lg h-10 w-10 active:scale-90"
      style="position: absolute; top: 10px; right: 10px;"
    >
      <Icon icon="material-symbols:cloud-download-outline" class="text-[1.5rem]"></Icon>
    </a>

    <div class="flex justify-center mt-4 space-x-2">
      <button on:click={onPrevPage} class="btn-regular px-3 rounded-lg active:scale-90" disabled={loading || pageNum <= 1}>
        <Icon icon="material-symbols:skip-previous-outline-rounded" class="text-[1.5rem]"></Icon>
      </button>
      <span class="px-4 py-2 whitespace-nowrap" style="cir">Page {pageNum} / {numPages}</span>
      <button on:click={onNextPage} class="btn-regular px-3 rounded-lg active:scale-90" disabled={loading || pageNum >= numPages}>
        <Icon icon="material-symbols:skip-next-outline-rounded" class="text-[1.5rem]"></Icon>
      </button>
    </div>
  {/if}
</div>

