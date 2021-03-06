# Revision history for reflex-platform

This project's release branch is `master`. This log is written from the
perspective of the release branch: when changes hit `master`, they are
considered released, and the date should reflect that release.

## Unreleased

* Bump `jsaddle*` to `0.9.7.0`.

* Test profiling in `release.nix`. Jobs names were shuffled in the process,
  which is acceptible as this file is not intended to be comsumed by anything
  but CI and QA. Add a note explaining that instability.
  
* Bump `reflex-todomvc`

## v0.4.1.0 - 2020-01-02

* Make Nixpkgs treat GHCJS builds as cross.
  This will fix things like `setup-depends` and `build-tool-depends` being properly built with GHC rather than GHCJS.
  That, in turn, will allow `markdown-unlit` READMEs and other things that we had trouble with.

* Bump cabal-macosx to correctly use the host platform rather than build
  platform to decide whether its appropriate to make a desktop app bundle.

## v0.4.0.0 - 2019-12-31

* Bump nixpkgs to 19.09.
  This will bump the versions of numerous packages, including much of Hackage.

## v0.3.0.0 - 2019-12-30

* Bump ghcjs-dom packages:

   - `ghcjs-dom` to `0.9.4.0`
   - `ghcjs-dom-jsaddle` to `0.9.4.0`
   - `ghcjs-dom-jsffi` to `0.9.4.0`
   - `jsaddle-dom` to `0.9.4.0`

* Bump aeson, and other packages to work with it:

   - `aeson` to `1.4.5.0`
   - `time-compat` to `1.9.2.2`
   - `hpack` to `0.32.0`
   - `webdriver` to `0.9.0.1`

* Bump ghc to include more `8.6` backports

## v0.2.0.0 - 2019-12-30

* Upgrade reflex packages:

   - `reflex` to `0.6.3`
   - `reflex-dom` to `0.5.3`
   - `reflex-dom-core` to `0.5.3`

* Document how to accept android sdk license agreement and pass acceptance
  through to android infrastructure.

* Update to GHC(JS) 8.6.5
  * Apply workaround patch for
    [ghc#16893](https://gitlab.haskell.org/ghc/ghc/issues/16893),
    avoiding segmentation fault when using base.

* Update to the nixos-19.03 nixpkgs channel

* Update to gradle build tools 3.1.0, androidsdk 9, and default to android
  platform version 28

* Bump reflex-dom 0.5.2

* Fixes an inconsistency between nix-shell and nix-build where certain
  Haskell build tools were not being overriden
  ([#548](https://github.com/reflex-frp/reflex-platform/pull/548)

* Removing long-since-broken `nixpkgs.haskell.compiler.ghcSplices` attribute,
  leaving behind `nixpkgs.haskell.compiler.ghcSplices-8_6`, which is its
  intended replacement.

* Add optional `runtimeSharedLibs` parameter to `buildApp`.
  See that function's documentation for how to use it.

* `zlib` for mobile doesn't provide any `*.so`/`.dylib`, that way you are
  guaranteed to link the `.a` and not one of the others by mistake.

* Fix work-on-multi to respect the do-check of the haskell-config derivations

* The following attributes have been deprecated in `default.nix`, and are now
  defined in `nix-utils/hackage.nix`:

   * `attrsToList`
   * `mapSet`
   * `mkSdist`
   * `sdists`
   * `mkHackageDocs`
   * `hackageDocs`
   * `mkReleaseCandidate`
   * `releaseCandidates`

  These are only useful to the maintainers of packages in reflex platform, and
  just clutter the top level for everyone else.

* Deprecate:

   * `generalDevTools`
   * `generalDevToolsAttrs`
   * `nativeHaskellPackages`

  And make `generalDevTools'` as a replacement for just `generalDevToolsAttrs`
  with a more structured argument.

  The are exposed for now for backwards compat, but will be removed at a later
  point. They are mainly used in the implementation of the `work-on*` scripts.

* The following attributes have been deprecated in `default.nix`, and are now
  defined in `nix-utils/work-on-multi.nix`:

   * `workOnMulti`
   * `workOnMulti'`

  If you just used `script/work-on-multi`, nothing has changed.
  But if you do use the nix attributes directly, see how the deprecated exports
  are written to migrate your code to not use them.

## v0.1.0.0 - 2019-04-03

* Move git "thunks" to `dep/` dirs within each overlay, rather than one big
  `dep/` directory at the top level. This does make checking out thunks a bit
  more involved, but has the benefit of untangling reflex-platform a bit and
  making it more modular.

* Many of the overlays have a `_dep` attribute with all the source derivations
  introduced by the overlay. This is just so `release.nix` can build all the
  sources, ensuring that these build-time dependencies also make it to the
  cache. This attribute is *not* intended to be overridden by consumers of
  reflex-platform. In particular it's possible the way we now push to the cache
  doesn't need this, and it will be removed entirely.

* Remove support for GHCs older than 8.4.
