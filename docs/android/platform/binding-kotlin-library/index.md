---
title: "Bind Android Kotlin libraries"
description: "This document describes how to create C# bindings to Kotlin code, making it possible to consume native libraries in a Xamarin.Android application."
ms.prod: xamarin
ms.assetid: AB03A6C4-5A5A-4EAD-AD51-D887B20A3551
ms.technology: xamarin-android
author: alexeystrakh
ms.author: dabritch
ms.date: 02/11/2020
---

# Bind Android Kotlin libraries

> [!IMPORTANT]
> We're currently investigating custom binding usage on the Xamarin platform. Please take [**this survey**](https://www.surveymonkey.com/r/KKBHNLT) to inform future development efforts.

The Android platform, along with its native languages and tooling, is constantly evolving and there are plenty of third-party libraries that have been developed using the latest offerings. Maximizing code and component reuse is one of the key goals of cross-platform development. The ability to reuse components built with Kotlin has become increasingly important to Xamarin developers as their popularity amongst developers continues to grow. You may already be familiar with the process of binding regular [Java](../binding-java-library/index.md) libraries. Additional documentation is now available describing the process of [Binding a Kotlin Library](walkthrough.md), so they are consumable by a Xamarin application in the same manner. The purpose of this document is to describe a high-level approach to create a Kotlin Binding for Xamarin.

## High-level approach

With Xamarin, you can bind any third-party native library to be consumable by a Xamarin application. Kotlin is the new language and creating binding for libraries built with this language requires some additional steps and tooling. This approach involves the following four steps:

1. Build the native library
1. Prepare the Xamarin metadata, which enables Xamarin tooling to generate C# classes
1. Build a Xamarin Binding Library using the native library and the metadata
1. Consume the Xamarin Binding Library in a Xamarin application

The following sections outline these steps with additional details.

### Build the native library

The first step is to obtain a native Kotlin library (AAR package, which is an Android archive). You can either request it directly from a vendor or build it yourself.

### Prepare the Xamarin metadata

The second step is to prepare the metadata transform file, which will be used by the Xamarin tools to generate the respective C# classes. In the best case scenario, this file could be empty where all classes are discovered and generated by the Xamarin tools. In some cases, metadata transformation must be applied to generate correct and/or desired C# code. In many cases, an AAR disassembler, such as [Java Decompiler (JD)](http://java-decompiler.github.io/), must be used to identify hidden dependencies and unwanted classes that you wish to exclude from the final list of C# classes to be generated. The final metadata should represent the public interface in which the referencing Xamarin.Android application will interact with.

### Build a Xamarin.Android binding library

The third step is to create a special project - a Xamarin.Android Binding Library. It includes the Kotlin libraries as native references and the metadata transformation defined in the previous step. At time of writing, a separate Android Binding Library project is required for each AAR package being referenced. The Binding Library must add the [Xamarin.Kotlin.StdLib](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/) package in order to support the Kotlin Standard Library.

### Consume the Xamarin binding library

The fourth and the final step is to reference the binding library in a Xamarin.Android application. Adding a reference to the Xamarin.Android Binding Library enables your Xamarin application to use the exposed Kotlin classes from within that package.

## Walkthrough

The approach above outlines the high-level steps required to create a Kotlin Binding for Xamarin. There are many lower-level steps involved and further details to consider when preparing these bindings in practice including adapting to changes in the native tools and languages. The intent is to help you to gain a deeper understanding of this concept and the high-level steps involved in this process. For a detailed step-by-step guide, refer to the [Xamarin Kotlin Binding Walkthrough](walkthrough.md) documentation.

## Related links

- [Android Studio](https://developer.android.com/studio)
- [Gradle Installation](https://gradle.org/install/)
- [Visual Studio for Mac](https://visualstudio.microsoft.com/downloads)
- [Java Decompiler](http://java-decompiler.github.io/)
- [BubblePicker Kotlin Library](https://github.com/igalata/Bubble-Picker)
- [Binding Java Library](../binding-java-library/index.md)
- [XPath](https://www.w3.org/TR/xpath/)
- [Java Binding Metadata](../binding-java-library/customizing-bindings/java-bindings-metadata.md)
- [Xamarin.Kotlin.StdLib NuGet](https://www.nuget.org/packages/Xamarin.Kotlin.StdLib/)
- [Sample project repository](https://github.com/xamcat/xamarin-binding-kotlin-framework)
