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

  await Promise.all([
    loadScript('https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js'),
    loadScript('https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js'),
  ]);

  const { pdfjsLib } = globalThis;
  pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js';

  pdfDoc = await pdfjsLib.getDocument(src).promise;
  numPages = pdfDoc.numPages;
  renderPage(pageNum);
});
</script>

<div class="relative rounded-md overflow-hidden" style="border: 1px solid var(--primary);">
  <canvas bind:this={canvas} style="width: 100%; aspect-ratio: 1/1.414;"></canvas>
  <a
    href={src}
    download
    class="btn-regular rounded-lg h-10 w-10 active:scale-90"
    style="position: absolute; top: 10px; right: 10px;"
  >
    <Icon icon="material-symbols:cloud-download-outline" class="text-[1.5rem]"></Icon>
  </a>
</div>

<div class="flex justify-center mt-4 space-x-2 absolute bottom-8 left-1/2 transform -translate-x-1/2">
  <button on:click={onPrevPage} class="btn-regular px-3 rounded-lg active:scale-90">
    <Icon icon="material-symbols:skip-previous-outline-rounded" class="text-[1.5rem]"></Icon>
  </button>
  <span class="px-4 py-2 text-black">Page {pageNum} / {numPages}</span>
  <button on:click={onNextPage} class="btn-regular px-3 rounded-lg active:scale-90">
    <Icon icon="material-symbols:skip-next-outline-rounded" class="text-[1.5rem]"></Icon>
  </button>
</div>
