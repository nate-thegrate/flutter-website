---
title: Migrating splash effects to the widgets library
description: >-
  "Splash" effects, which previously relied on a MaterialInkController from a
  Material widget, now use the SplashController from a SplashBox.
---

## Summary

"Splash" effects, which previously relied on a `MaterialInkController` from a
`Material` widget, now use the `SplashController` from a `SplashBox`.

## Background

Previously, `MaterialInkController` was exclusive to the `Material` widget.
The [Ink effects in `widgets` library][] pull request added a `SplashBox`
widget that enables splash effects while remaining "design system-agnostic",
so now the `SplashController` is no longer tied to `Material`.

Breaking changes include renaming the following:

| Original names                         | "Splash" names     |
|----------------------------------------|--------------------|
| `InkFeature` / `InteractiveInkFeature` | `Splash`           |
| `InteractiveInkFeatureFactory`         | `SplashFactory`    |
| `MaterialInkController`                | `SplashController` |
| `debugCheckHasMaterial`                | `debugCheckSplash` |

Additionally, `Material` was changed from a `StatefulWidget` to a `StatelessWidget`.
(Its runtime behavior is unaffected.)

## Migration guide

A [Flutter fix][] is available to automatically update the names of existing
classes and functions.

To migrate, rename each instance of `debugCheckHasMaterial` to `debugCheckSplash`,
and change other class names as per the table above.

There are no changes to the `Material` widget's runtime behavior, but any tests
that expect `Material` to be a `StatefulWidget` should be adjusted to accept a
`StatelessWidget`.

Code before migration:

```dart
assert(debugCheckHasMaterial(context));
final MaterialInkController controller = Material.of(context);

class MyEffect extends InteractiveInkFeature {}
```

Code after migration:

```dart
assert(debugCheckSplash(context));
final SplashController controller = Material.of(context);

class MyEffect extends Splash {}
```

## Timeline

Landed in version: xxx<br>
In stable release: Not yet

## References

Relevant issues:

* [Equivalent of `InkWell` in Cupertino style][]
* [A simple `MaterialInkController` widget][]
* [Easier, more direct access to ink effects][]


Relevant PRs:

* [Ink effects in `widgets` library][]

[Equivalent of `InkWell` in Cupertino style]: {{site.repo.flutter}}/issues/48017
[A simple `MaterialInkController` widget]: {{site.repo.flutter}}/issues/147392
[Easier, more direct access to ink effects]: {{site.repo.flutter}}/issues/150139
[Flutter fix]: /tools/flutter-fix
[Ink effects in `widgets` library]: {{site.repo.flutter}}/pull/149963