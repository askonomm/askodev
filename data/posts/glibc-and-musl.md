---
title: GLIBC and MUSL
slug: glibc-and-musl
date: 2023-10-17
status: published
---

So there I was, trying to get my [static site generator](https://github.com/askonomm/vau) to run on [Vercel](https://vercel.com/) when I was greeted with an error:

```
/lib64/libc.so.6: version `GLIBC_2.28' not found
```

Hmm. It worked on my machine! Well, a little research revealed to me that Rust executables aren't actually fully static, meaning they
rely on GLIBC to work and hence there is a problem since, I assume, Vercel does not have GLIBC installed. 

Researching more I found that the solution is [MUSL](https://dev-doc.rust-lang.org/beta/edition-guide/rust-2018/platform-and-target-support/musl-support-for-fully-static-binaries.html), 
which allows to build a fully static binary. I wanted to test it on my local machine before adding it to the 
[build pipeline](https://github.com/askonomm/vau/blob/main/.github/workflows/release.yml), but getting `musl-gcc` on macOS (M2) proved too difficult to install for me to care, so I just added it straight
to the build pipeline instead and voilà, now the executable runs on Vercel as well. 