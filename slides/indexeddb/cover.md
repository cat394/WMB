---
title: IndexedDB tutorial
---

# <span class="text-white">{{$slidev.configs.title }}</span>

Efficient local storage

<div class="pt-12">
  <span class="px-2 py-1 rounded cursor-pointer transition-all border border-transparent">
   <span>What is indexedDB?</span><carbon-arrow-right class="inline-block pl-1 moveRight-and-back" />
  </span>
</div>

<div class="abs-br m-6 flex gap-2">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" alt="GitHub" title="Open in GitHub"
    class="text-xl slidev-icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

<div class="background-image absolute inset-0 z-[-1]"></div>

<style>
  .background-image {
    background-image: -webkit-cross-fade(url("./cover.avif"), linear-gradient(315deg, #2d3436 0%, #000000 74%), 60%);
    background-size: cover;
  }
</style>