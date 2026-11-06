# Deadlock mod storage page

The web page that Deadlock mods made with [deadlock-ai-mod-kit](https://github.com/UnExploration/deadlock-ai-mod-kit) save through. It is served by GitHub Pages at:

```
https://unexploration.github.io/deadlock-mod-storage/
```

Nothing is stored in this repo, ever. It holds one web page, and the page holds no data.

## Why a web page

Deadlock mods are client-side UI and asset changes. They cannot write files, so a mod that wants to remember something, a note about a player, a mark, a setting, has nowhere to put it. The one place a mod can reach that survives a restart is the game's built-in web browser, which keeps its own storage like any browser does.

So a mod that saves data loads this page invisibly when the game starts and hands it the things to remember. The page keeps them in the browser's storage on the player's own PC. It makes no network requests and runs no code from anywhere else. Open the address above in a normal browser and you see the same explanation; the page does nothing on its own.

## Where the data is

- On the player's PC, in Steam's web browser storage, filed under this page's address.
- Never sent anywhere. The page has no server side and cannot reach the network.
- Each mod's data sits under the mod's own name. Mods cannot read each other's data.
- Clearing Steam's browser data (Steam > Settings > In Game > Delete Web Browser Data) wipes it.

Because the browser files the data under the page's address, a mod only finds what it saved for as long as it keeps using the same address.

## The warning every mod maker gets

The kit's storage page is run by the kit's author, free, with no guarantee it stays online. If it goes away, mods that use it stop saving until their maker hosts the page at an address of their own, and data players saved under the old address does not carry over.

## For mod makers

**Using this page.** The kit asks once, when it makes a mod that saves data, which address the mod saves through. Answering with the kit's page means this address. The mod gets it written into `mod.json` and one small script, and it should never be changed afterwards: players' saved data stays with the address it was saved under.

**Hosting your own copy.** See the next section. Your mods then save through your address instead of this one, and nobody but you decides how long it stays up.

**The page's source** lives in the kit at `store/index.html`, with its tests. This repo carries a copy of that file. When the page changes in the kit, the copy here is updated to match.

## Make your own copy

This repo is a GitHub template. Three ways to get your own storage page, all free; pick one.

**1. The button, about two minutes.** Needs a GitHub account.

1. Click **Use this template** at the top of this page, then **Create a new repository**. Name it, for example, `deadlock-mod-storage`. Keep it **public**: GitHub Pages is free only for public repos.
2. In the new repo, open **Settings > Pages**. Under **Build and deployment**, set Source to **Deploy from a branch**, pick `main` and `/ (root)`, and save.
3. Wait one to ten minutes. Your page is at `https://<your GitHub name>.github.io/<repo name>/`, with the trailing slash. Open it in a browser: you should see the same explanation this page shows.

**2. One command from the kit.** With the kit's tools and the GitHub CLI installed and signed in:

```
pwsh -File tools/store.ps1 -Mod mods/<your mod> -Publish
```

It creates a public repo under your account, puts this page in it, turns on Pages, waits until the address answers and writes it into your mod.

**3. By hand, on any host.** Any static `https://` host works: put `index.html` at the root (and, on GitHub, an empty `.nojekyll` next to it), and use the address with a trailing slash.

**Then point your mods at it.** When the kit asks which address a new mod saves through, give yours. Once a mod has been shared, keep its address forever: moving it starts players over with nothing. If this page's file changes in the kit later, copy the new `index.html` into your repo; the protocol is versioned, so older mods keep working.

Your copy carries the same warning for your players, with you as the one running it: free, and up for as long as you keep it up.

## How the page talks to a mod

For the curious. The mod loads the page in a hidden browser panel and sends each request in the address's fragment (the part after `#`), which never leaves the browser. The page answers by setting its title, which the game hands back to the mod. Every request names the mod's namespace, like `notepad.v1`, checked against a strict pattern, and can only touch keys under that namespace. Operations: `hello`, `get`, `set`, `del`, `list`. Values are capped at about 3000 characters each; the browser gives one address about 5 MB in total. The protocol is versioned, and the full details are in the kit's `docs/04-persistence-and-io.md`.

## Licence

MIT, like the kit. Unofficial and not affiliated with Valve.
