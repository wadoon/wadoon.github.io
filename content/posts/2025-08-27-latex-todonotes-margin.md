---
title: "LaTeX: More margin for \\todo"
date: 2025-09-27
---

Following lines increase the  `paperwidth` and `marginparwidth`. This gives `\todo` more space in the margin, w/o changing the original `textwidth` and `linewidth`:

```latex
\paperwidth=\dimexpr \paperwidth + 6cm\relax
%\oddsidemargin=\dimexpr\oddsidemargin + 3cm\relax
%\evensidemargin=\dimexpr\evensidemargin + 3cm\relax
\marginparwidth=\dimexpr \marginparwidth + 6cm\relax
```
