# Changelog

All notable changes to Dependency Checker Setup (`dev.terrorized.dependencycheckersetup`) and the
checker it installs (`Terrorized.DependencyChecker.dll`).

## 1.1.0 - unreleased

### Fixed
- **Several creators' checkers in one project no longer break it.** A buyer who owns avatars from
  more than one creator used to get "Multiple precompiled assemblies with the same name" (script
  compilation blocked) or a duplicate `DependencyCheckerBootstrap` class error. Now:
  - the checker library ships with a fixed GUID, so importing several packages leaves one copy;
  - each checker gets its own bootstrap file and class (`DependencyCheckerBootstrap_<id>.cs`),
    with a GUID derived from its id, so re-importing the same product updates it in place;
  - every checker keeps its own settings, menu, window and window title;
  - each creator's splash and custom icons load by asset GUID, so one creator's art no longer
    shows up in another creator's checker.
- Deleting one creator's folder no longer breaks the other checkers: bootstraps reach the library
  through reflection and show a "re-import the package" message if it is missing.
- Installing a package can no longer freeze Unity. vrc-get output is read asynchronously, every
  run has a 5 minute timeout, and a cancelable progress bar shows while it works.
- The checker opens once per editor session instead of after every script recompile.
- "Show window on load" is saved per project and per checker. Turning it off for one checker no
  longer turns off every checker on the computer.
- Running Download & Install on an existing install now really updates it: the newer library is
  downloaded and replaces the old one, missing shared files are added, and your own art is kept.
- The dependency list refreshes by itself once Unity has finished registering new packages.
- The avatar scene is found even when it comes from a package installed after the window opened.

### Added
- Optional **Product / Avatar** name, so one creator can ship a separate checker per product.
- **Switch** button on the Build Platform row (asks first, because switching re-imports the project).
- **Help** button on the Unity row when the version doesn't match, linking VRChat's current Unity
  version page.
- vrc-get downloads are checked against pinned SHA-256 hashes before they are run.
- When the setup tool finds a checker from 1.0 or another checker in the project, it asks whether
  to replace it or keep both.

### Changed
- vrc-get bumped from v1.9.1 to v1.9.2 (stable, 2026-07-12; enforces package hash checks).
- Creator art now lives in `DependencyChecker/Branding/` instead of `Resources/`. 1.0 installs are
  migrated when you run the setup tool again.
- The placeholder splash is now `Resources/DependencyChecker_DefaultSplash.png`.
- Dialog texts rewritten as plain sentences; the credit reads "Script made by Terrorized".

### Compatibility
- 1.0 bootstraps keep working with the 1.1 library (`DependencyCheckerRuntime.Configure` / `Open`).
  The build compiles a real 1.0 bootstrap against the new library to make sure.
- A 1.0 install still has a random library GUID, so it can still clash with another creator's
  checker in a buyer's project. Re-run the setup tool (1.1) and re-export your package to fix that.

## 1.0.0

- First release: DRM-gated setup tool that downloads the obfuscated checker, brands it with the
  creator's name, splash and socials, and generates one readable bootstrap file.
