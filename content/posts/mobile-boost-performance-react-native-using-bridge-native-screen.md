+++
title = 'Mobile: Boost Performance React Native Using Bridge Native Screen'
date = 2024-02-26T17:55:09+07:00
draft = false
+++

## Intro

The topic of native code bridges is an ongoing challenge, especially when React Native's latest documentation when this was written
are still marks some features as [**experimental**](https://reactnative.dev/docs/the-new-architecture/pillars-fabric-components). based on this reason i do research into bridging native screens into React Native's base code seems like a promising initiative. This could potentially serve as a pathway for developers to refactor towards pure native code, such as Java/Kotlin and Swift, without abandoning React Native side.

## Problem

> One of the challenges with managing multiplatform software development is the laborious efforts of creating & maintaining parity for every identical component constantly across platforms.

I think all React Native devs know the deal, screens built with React Native don't quite match up in performance with those crafted in pure native code, like Kotlin/Java and Swift

But it's a common struggle to deals with performance issue. With so much existing React Native codebases in companies, we're kinda stuck rolling with it. and gotta keep optimizing performance using tricks like **memoizing components**, **preventing unnecessary component updates**, **handling primitive data updates**, **caching**, and the whole shebang.

## Solution

For now, I'll be dividing this into several threads that you can read sequentially. This will serve as a flow to understand the correct sequence for implementing the Native Screen bridge.

- Mobile: Boost Performance React Native Create Your First Native Screen
  - Jetpack Compose
  - Swift UIKIT
- Mobile: Boost Performance React Native Integrate With Navigating Between Native Screen
- Mobile: Boost Performance React Native Call Your Cache Data Across Native Screen
  - Deals with react native async storage between native side
