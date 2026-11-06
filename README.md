# Transition Foundry · landing page

Live: <https://jiaaowang-ut.github.io/tsfoundry/> · 中文 <https://jiaaowang-ut.github.io/tsfoundry/zh.html> ·
TF-B1 benchmark report <https://jiaaowang-ut.github.io/tsfoundry/tf-b1/>
Custom domain: `tsfoundry.ai4mat.ai` (active once its DNS record exists, see below).

This repository holds only the generated static site. It is produced by `docs/tsfoundry/build.py` in the Transition
Foundry workspace: every number on the page is computed from the TF-B1 benchmark files (`tf-b1/`) and checked at build
time. Do not edit these files by hand; rebuild and redeploy instead.

## Switching to tsfoundry.ai4mat.ai

1. DNS (zone `ai4mat.ai`): add a `CNAME` record `tsfoundry` → `jiaaowang-ut.github.io`, proxy status **DNS only**.
2. In the workspace run `.venv/bin/python docs/tsfoundry/build.py --cname` (writes `CNAME` and switches the canonical,
   og:image, hreflang and sitemap URLs to the domain), copy `docs/tsfoundry/public/.` into this repository (keep this
   README), commit and push. GitHub Pages picks the domain up from the `CNAME` file.
3. When Settings → Pages reports the certificate as issued, tick **Enforce HTTPS**.

The Transition Foundry workspace (code and data) is private for now. Access and questions: wangjiaao0720@utexas.edu
