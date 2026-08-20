---
title: "tinyz: Private image compression in your browser"
published: 2026-08-20
description: "I built tinyz, a local-first image compressor that processes PNG, JPEG, and WebP files in your browser with Rust and WebAssembly."
image: ""
tags: ["Rust", "WebAssembly", "Svelte", "Image Tools", "Project"]
category: "Projects"
draft: false
lang: "en"
---

**[tinyz-two](https://tinyz-two.vercel.app)** is a small, local-first image compressor for the browser. Drop in a batch of PNG, JPEG, or WebP images, choose the quality and output format, and download the results individually or as a ZIP archive.

## How it works

The compressor runs in a Web Worker so image processing does not block the interface. The actual encoding code is written in Rust and compiled to WebAssembly. The page creates a small pool of workers for batches, while each worker keeps its own jobs ordered.

The Rust pipeline decodes the input image, applies its EXIF orientation when available, and then encodes it in the selected format:

- **PNG:** palette quantization with dithering.
- **JPEG:** quality-controlled RGB encoding with transparent pixels flattened onto white.
- **WebP:** palette quantization followed by lossless WebP encoding.

The result also includes the time spent in the compression pipeline, which makes the queue useful for seeing how the browser is handling a batch.

## A focused workflow

tinyz accepts multiple files at once. When the first image is added, the output format is detected from its file type. The quality slider and format selector can then be adjusted before running the batch again.

Each completed image gets a side-by-side comparison preview. You can drag the center handle, zoom in to inspect details, or open the preview fullscreen. The results list shows the original and compressed sizes, the reduction percentage, and the processing time.

Finished files can be downloaded one at a time, or the entire output queue can be packaged into a ZIP archive directly in the browser.

## Built with

- [SvelteKit](https://svelte.dev/)
- [Rust](https://www.rust-lang.org/) and [WebAssembly](https://webassembly.org/)
- [imagequant](https://pngquant.org/lib/) for palette quantization
- [fflate](https://github.com/101arrowz/fflate) for in-browser ZIP creation

The project is open source on [GitHub](https://github.com/neoju/tinyz). It is also a useful little experiment in keeping a browser utility fast, private, and understandable from the first interaction.

Give it a spin over at **[tinyz-two](https://tinyz-two.vercel.app)**, and I hope it makes your dev workflow a little smoother!
