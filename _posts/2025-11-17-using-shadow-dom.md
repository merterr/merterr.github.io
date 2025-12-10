---
layout: page
title: Shadow DOM Kullanımı
order: 4
---

## Shadow DOM Kullanımı

```html
<div id="my-shadow-host"></div>

<div id="hidden-content" style="display:none;">@Html.Raw(Model.Content)</div>
```

```js
<script>
    document.addEventListener("DOMContentLoaded", function () {

        const host = document.getElementById("my-shadow-host");
        const hidden = document.getElementById("hidden-content");

        if (!host || !hidden) return;

        // Shadow root oluştur
        const shadow = host.attachShadow({ mode: "open" });

        // İçeriği shadow’a bas
        shadow.innerHTML = `
            <style>
               /* Burada shadow root için özel stiller yazabilirsin */
               .info-content { padding: 10px; }
            </style>
            <div class="info-content">
                ${hidden.innerHTML}
            </div>
        `;
    });
</script>
```

```js
document.addEventListener("DOMContentLoaded", function () {
  const observer = new MutationObserver(() => {
    const liveChatAiEl = document.getElementById("live-chat-ai-host");

    if (liveChatAiEl) {
      const shadow = liveChatAiEl.shadowRoot;
      const btn = shadow.querySelector("#live-chat-ai-button");
      btn.style = "left: auto;right: 4.5rem;bottom: 5.5rem;";
      observer.disconnect();
    }
  });

  observer.observe(document.body, {
    childList: true,
    subtree: true,
  });
});
```
