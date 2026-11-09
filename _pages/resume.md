---
layout: page
permalink: /resume/
title: CV
description:
nav: true
nav_order: 3
---

<p>Last updated: November 2026 &nbsp;
  <a class="btn btn-sm z-depth-0" role="button" href="/assets/pdf/resume.pdf" target="_blank" rel="noopener" style="border: 1px solid var(--global-theme-color); color: var(--global-theme-color);">
    <i class="fa-solid fa-file-pdf"></i> Download PDF
  </a>
</p>

<div id="cv-pages" style="display: flex; flex-direction: column; gap: 1rem;">
  <p id="cv-loading">Loading CV&hellip; If it does not appear, <a href="/assets/pdf/resume.pdf">open the PDF directly</a>.</p>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
<script>
  (function () {
    var url = "/assets/pdf/resume.pdf";
    var container = document.getElementById("cv-pages");
    var loading = document.getElementById("cv-loading");
    if (!window.pdfjsLib) return;
    pdfjsLib.GlobalWorkerOptions.workerSrc = "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js";

    pdfjsLib.getDocument(url).promise.then(function (pdf) {
      var ratio = window.devicePixelRatio || 1;
      var width = container.clientWidth;
      var pages = [];
      for (var i = 1; i <= pdf.numPages; i++) pages.push(pdf.getPage(i));
      return Promise.all(pages).then(function (list) {
        loading.remove();
        return Promise.all(list.map(function (page) {
          var base = page.getViewport({ scale: 1 });
          var viewport = page.getViewport({ scale: (width / base.width) * ratio });
          var wrap = document.createElement("div");
          wrap.style.position = "relative";
          wrap.style.boxShadow = "0 1px 6px rgba(0,0,0,0.15)";
          var canvas = document.createElement("canvas");
          canvas.width = viewport.width;
          canvas.height = viewport.height;
          canvas.style.width = "100%";
          canvas.style.display = "block";
          canvas.style.background = "#fff";
          wrap.appendChild(canvas);
          container.appendChild(wrap);
          // keep PDF hyperlinks clickable
          page.getAnnotations().then(function (annots) {
            var w = base.width, h = base.height;
            annots.forEach(function (a) {
              if (a.subtype !== "Link" || !a.url) return;
              var link = document.createElement("a");
              link.href = a.url;
              link.target = "_blank";
              link.rel = "noopener";
              link.style.position = "absolute";
              link.style.left = (a.rect[0] / w) * 100 + "%";
              link.style.top = ((h - a.rect[3]) / h) * 100 + "%";
              link.style.width = ((a.rect[2] - a.rect[0]) / w) * 100 + "%";
              link.style.height = ((a.rect[3] - a.rect[1]) / h) * 100 + "%";
              wrap.appendChild(link);
            });
          });
          return page.render({ canvasContext: canvas.getContext("2d"), viewport: viewport }).promise;
        }));
      });
    }).catch(function () {
      loading.innerHTML = 'Could not display the CV here. <a href="' + url + '">Open the PDF</a>.';
    });
  })();
</script>
