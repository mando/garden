---
{"dg-publish":true,"permalink":"/tech/homebrew-sneaks/","title":"`homebrew` sneaks","tags":["TIL"],"created":"2025-10-12T16:10:50.817-05:00"}
---

# `homebrew` sneaks

I think I've used `homebrew` for about a million years, but only today I learned that you can use `brew install` as an alias for `brew upgrade`:

```
❯ brew outdated | grep docker
docker-compose
~/code/notas
❯ brew install docker-compose
docker-compose 2.39.3 is already installed but outdated (so it will be upgraded).
```

Never again will I accidentally try to update `homebrew` instead of updating a package:

```
❯ brew update docker-compose
Error: This command updates brew itself, and does not take formula names.
Use `brew upgrade docker-compose` instead.
```

Whoever did this is my hero and I will love you forever