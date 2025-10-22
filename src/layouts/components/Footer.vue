<template>
  <div class="h-100 d-flex align-center justify-space-between">
    <!-- 👉 Footer: left content -->
    <span class="d-flex align-center">
      &copy;
      {{ new Date().getFullYear() }} он
      Хийсэн
       <a
        href="https://123.com"
        target="_blank"
        rel="noopener noreferrer"
        class="text-primary ms-1"
      >SANTANA</a>
    </span>
    <!-- 👉 Footer: right content -->
    <span class="d-md-flex gap-x-4 text-primary d-none">
      <a
        href="https://www.facebook.com/"
        target="noopener noreferrer"
      >Facebook</a>
      <a
        href="https://www.twitch.tv/"
        target="noopener noreferrer"
      >Twitch</a>
      <a
        href="https://kick.com/"
        target="noopener noreferrer"
      >Kick</a>
    </span>
  </div>
</template>
