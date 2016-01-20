# Browserify transform scopes

This repository aims to show you how browserify applies transforms. There are two important scopes to distinguish:

1. *Package scope*  
Browserify will apply the transform you defined for the package it was defined in only.
2. *App scope*  
Browserify will apply the transform to all of the dependencies it encounters when bundling.

# Examples

Prior to the examples below, run `npm install`

## Shim `jquery` in current package

`browserify index.js > bundle.js -t browserify-shim`

In the `package.json` of this project we have:

```
"browserify-shim": {
    "jquery": "global:jQuery"
}

// Instead of specifying the transform in the browserify command, you could also have it in the package.json:

"browserify": {
    "transform": [ "browserify-shim" ]
}
```

*run `npm run shim:pkg` to inspect what is included in the `bundle.js` created by browserify.  
Expected result: jQuery is in the bundle! (because one of the dependencies relies on it)*

## Shim `jquery` in all dependencies

`browserify index.js > bundle.js -t [ browserify-shim --global ]`

In the `package.json` of this project we have:

```
"browserify-shim": {
    "jquery": "global:jQuery"
}

// Note that you cannot specify global transforms inside `package.json`. You have to use the command!

```

Applies the transform config defined in the current `package.json` to *ALL* dependencies.

*run `npm run shim:app` to inspect what is included in the `bundle.js` created by browserify.  
Expected result: jQuery is __NOT__ the bundle!*
