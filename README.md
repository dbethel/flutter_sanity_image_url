# Flutter Sanity Image URL (Fork)

![GitHub](https://img.shields.io/badge/source-GitHub-blue)
![Version](https://img.shields.io/badge/version-0.1.0-green)

A Flutter package for generating Sanity image URLs with advanced image handling capabilities.

**Note: This is a fork of the original `flutter_sanity_image_url` package.** This version includes additional features and fixes that are not available in the published version. Since this fork is not published to pub.dev, it must be installed directly from GitHub.

## Original Attribution

This package is originally ported from [sanity-io/image-url](https://github.com/sanity-io/image-url) and is intended to be used together with the [flutter_sanity package](https://pub.dev/packages/flutter_sanity).

## Why This Fork?

This fork exists to provide additional features, bug fixes, or improvements that may not be available in the original published package. Since the changes are specific to certain use cases and may not be suitable for the main package, this fork is maintained separately.

## Features

Easily make use of all of the image related features provided by Sanity:

- Allow editors of content to specify crops and hotspots, which will be respected in the app by creating the correct url.
- Apply image transformations, like setting the width and height of an image through a `ImageUrlBuilder`.
- Provides a `SanityImage.fromJson()` method that parses the image data from sanity into an object with all data and metadata being typed:
  - Access the sanity image color palette, for better styling based on images.
  - Access to `lqip` to implement low resolution image placeholders.

<p align="center">
    <img width="200px" src="https://raw.githubusercontent.com/dbethel/flutter_sanity_image_url/main/assets/screenshot.png"/>
</p>

## Installation

Since this is a fork and not published to pub.dev, you need to install it directly from GitHub.

Add the following to your `pubspec.yaml` file:

```yaml
dependencies:
  flutter_sanity: ^any # Install from pub.dev
  flutter_sanity_image_url:
    git:
      url: https://github.com/dbethel/flutter_sanity_image_url.git
      ref: main # or specify a specific branch/tag
```

Then run:

```bash
flutter pub get
```

## Usage

See `/example` for a full example.

### Displaying an Image:

1. create an instance of `ImageUrlBuilder` which you can then use throughout your app.
2. get an image url by chaining calls to the `ImageUrlBuilder`.

#### 1. Creating a builder:

```dart
// sanityClient is an instance of SanityClient from flutter_sanity
final builder = ImageUrlBuilder(sanityClient);

ImageUrlBuilder urlFor(asset) {
  return builder.image(asset);
}
```

#### 2. Using the builder:

using the builder design patten the options can be added in a chain, always call `url()` at the end to get the actual url of the image.

```dart
Image.network(urlFor(image).size(200, 200).url())
```

### Accessing the image color palette data

Available colors in the pallette:

- `darkMuted`
- `darkVibrant`
- `dominant`
- `lightMuted`
- `lightVibrant`
- `muted`
- `vibrant`

Each has color has the attributes:

- `background`
- `foreground`
- `title`

Example of using the color pallette for an image:

```dart
print(myImage.palette?.darkMuted.background);
// output: Color(0xff2c3b52)
```

See the example project for an example of styling text and background overlays based on the color pallette.

### Accessing Low Quality Image Previews for low resolution image placeholders.

> `LQIP` (Low-Quality Image Preview) is a 20-pixel wide version of your image (height is set according to aspect ratio) in the form of a base64-encoded string.

By using the `flutter_sanity_image` package you can use Low Quality Image Previews in two ways:

#### Use the provided placeholder widget:

```dart
ImagePlaceholder(lqip: pictures[0].lqip);
```

#### Access the lqip directly to implement into your own widget:

```dart
print(myImage.lqip)
/*
2wBDAAYEBQYFBAYGBQYHBwYIChAKCgkJChQODwwQFxQYGBcUFhYaHSUfGhsjHBYWICwgIyYnKSopGR8tMC0oMCUoKSj/2wBDAQcHBwoIChMKChMoGhYaKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCgoKCj/wAARCAANABQDASIAAhEBAxEB/8QAFwAAAwEAAAAAAAAAAAAAAAAAAAYHBf/EACYQAAIBAwIEBwAAAAAAAAAAAAECAwAEBREhBgcVMhITFjFBYZH/xAAWAQEBAQAAAAAAAAAAAAAAAAAEAQL/xAAaEQEAAgMBAAAAAAAAAAAAAAABAAIREkFR/9oADAMBAAIRAxEAPwBl5ZX11aYSWNoAYUGsKL861u+rlRX6pD5ECA+NgdhUoXi2+wuIjjtghdx3n32pMyufyOZuNbyclSewbL+UuplVh7OACUnN8wNcjJ0mGOS0GyuynVvuip/FG0kYKysgGwAore1TkmtvZ//Z
*/
```
