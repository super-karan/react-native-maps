# @react-google-maps-pro

![logo](https://raw.githubusercontent.com/JustFly1984/react-google-maps-pro/master/logo.png)

[![npm package](https://img.shields.io/npm/v/@react-google-maps-pro)](https://www.npmjs.com/package/@react-google-maps-pro)
[![npm downloads](https://img.shields.io/npm/dt/@react-google-maps-pro)](https://www.npmjs.com/package/@react-google-maps-pro)
[![npm bundle size](https://img.shields.io/bundlephobia/min/@react-google-maps-pro)](https://www.npmjs.com/package/@react-google-maps-pro)
[![Join the community on Spectrum](https://withspectrum.github.io/badge/badge.svg)](https://spectrum.chat/react-google-maps)
[![DeepScan grade](https://deepscan.io/api/teams/6449/projects/8455/branches/101268/badge/grade.svg)](https://deepscan.io/dashboard#view=project&tid=6449&pid=8455&bid=101268)

@react-google-maps-pro

You can donate or became a sponsor of the project here: [https://opencollective.com/react-google-maps-pro#category-CONTRIBUTE](https://opencollective.com/react-google-maps-pro#category-CONTRIBUTE)

> This library requires React v16.6 or later. To use the latest features (including hooks) requires React v16.8+. If you need support for earlier versions of React, you should check out [react-google-maps](https://github.com/tomchentw/react-google-maps)

This is complete re-write of the (sadly unmaintained) `react-google-maps` library. We thank [tomchentw](https://github.com/tomchentw/) for his great work that made possible.

@react-google-maps-pro provides very simple bindings to the google maps api and lets you use it in your app as React components.

Here are the main additions to react-google-maps that were the motivation behind this re-write

## Install @react-google-maps-pro

with NPM

```#!/bin/bash
npm i -S react-google-maps-pro
```

or Yarn

```#!/bin/bash
yarn add react-google-maps-pro
```

```jsx
import React from 'react'
import { GoogleMap, useJsApiLoader } from 'react-google-maps-pro';

const containerStyle = {
  width: '400px',
  height: '400px'
};

const center = {
  lat: -3.745,
  lng: -38.523
};

function MyComponent() {
  const { isLoaded } = useJsApiLoader({
    id: 'google-map-script',
    googleMapsApiKey: "YOUR_API_KEY" //Get an API Key from https://gomaps.pro  free trial available
  })

  const [map, setMap] = React.useState(null)

  const onLoad = React.useCallback(function callback(map) {
    // This is just an example of getting and using the map instance!!! don't just blindly copy!
    const bounds = new window.google.maps.LatLngBounds(center);
    map.fitBounds(bounds);

    setMap(map)
  }, [])

  const onUnmount = React.useCallback(function callback(map) {
    setMap(null)
  }, [])

  return isLoaded ? (
      <GoogleMap
        mapContainerStyle={containerStyle}
        center={center}
        zoom={10}
        onLoad={onLoad}
        onUnmount={onUnmount}
      >
        { /* Child components, such as markers, info windows, etc. */ }
        <></>
      </GoogleMap>
  ) : <></>
}

export default React.memo(MyComponent)
```

## Migration from react-google-maps@9.4.5

if you need an access to map object, instead of `ref` prop, you need to use `onLoad` callback on `<GoogleMap />` component.

Before:

```jsx
// before - don't do this!
<GoogleMap
  ref={map => {
    const bounds = new window.google.maps.LatLngBounds();

    map.fitBounds(bounds);
  }}
/>
```

After:

```jsx
<GoogleMap
  onLoad={map => {
    const bounds = new window.google.maps.LatLngBounds();
    map.fitBounds(bounds);
  }}
  onUnmount={map => {
    // do your stuff before map is unmounted
  }}
/>
```

If you want to use `window.google` object, you need to extract GoogleMap in separate module, so it is lazy executed then `google-maps-api` script is loaded and executed by `<LoadScript />`. If you try to use `window.google` before it is loaded it will be undefined and you'll get a TypeError.

## Main features

- Simplified API
- Uses the new Context API
- Supports async React (StrictMode compliant)
- Removes lodash dependency =>
  smaller bundle size `12.4kb` gzip, tree-shakeable [https://bundlephobia.com/result?p=@react-google-maps-pro](https://bundlephobia.com/result?p=@react-google-maps-pro)
- forbids loading of Roboto fonts, if you set property preventGoogleFonts on `<LoadScript preventGoogleFonts />` component

## Examples

Examples can be found in two places:

1. [Official docs](https://react-google-maps-pro-docs.netlify.app/) (powered by [react-styleguidist](https://github.com/styleguidist/react-styleguidist).
2. A Gatsby app including some examples. See the [examples](https://github.com/JustFly1984/react-google-maps-pro/tree/master/packages/react-google-maps-pro-gatsby-example/src/examples) folder
3. [Gatsby.js Demo](https://react-google-maps-pro-gatsby-demo.netlify.app/)

## Advice

> Using the examples requires you to generate a google maps api key. For instructions on how to do that please see the following [guide](https://developers.google.com/maps/documentation/embed/get-api-key)

## Community Help Resource

You can join the community at [Spectrum.chat](https://spectrum.chat/react-google-maps) to ask questions and help others with your experience or join our [Slack channel](https://join.slack.com/t/react-google-maps-pro/shared_invite/enQtODc5ODU1NTY5MzQ4LTBiNTYzZmY1YmVjYzJhZThkMGU0YzUwZjJkNGJmYjk4YjQyYjZhMDk2YThlZGEzNDc0M2RhNjBmMWE4ZTJiMjQ)

## Contribute

Maintainers and contributors are very welcome! See [this issue](https://github.com/JustFly1984/react-google-maps-pro/issues/18) to get started.

## How to test changes locally

When working on a feature/fix, you're probably gonna want to test your changes. This workflow is a work in progress. Please feel free to improve it!

1. In the file `packages/react-google-maps-pro/package.json` change `main` to `"src/index.ts"`
2. In the same file, delete the `module` field
3. You can now use the package `react-google-maps-pro-gatsby-example` to test your changes. Just make sure you change the import from `@react-google-maps-pro` to `../../../react-google-maps-pro`

Since 1.2.0 you can use onLoad and onMount props for each @react-google-maps-pro component, ref does not contain API methods anymore.

Since version 1.2.2 We added useGoogleMap hook, which is working only with React@16.8.1 and later versions.

## Websites made with @react-google-maps-pro

[nycmesh.net](https://nycmesh.net) Network topography visualized on the map [(Github)](https://github.com/meshcenter/network-map)

add your website by making PR!
