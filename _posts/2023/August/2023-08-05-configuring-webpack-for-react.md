---
title: Configuring Webpack and other tooling for React [Part-1]
tags: [React, webpack5, javascript]
description: Setting up react development environment using webpack 5
comments: true
mathjax: false
style: fill
color: info
---

In this post, we'll be setting up the development environment for a react application environment which uses various bits and pieces in synergy. We'll be setting up webpack5 environment along with jest, react-testing-library, babel, husky, linting and much more altogether to create a boilerplate application which can be used as a placeholder for spinning up the production grade react app.

## A sneak peek into the folder structure

```sh
|-- .husky
    |-- _
        |   +-- .gitignore
        |   +-- husky.sh
    |   +-- pre-commit
|-- src
    |-- components
        |   +-- App.js
        |   +-- App1.js
        |   +-- App1.test.js
        |   +-- Recipes.jsx
        |   +-- Recipes.test.js
    |-- images
        |   +-- image1.gif
        |   +-- image2.jpg
        |   +-- image3.png
        |   +-- image4.png
        |   +-- image5.svg
    |-- styles
        |   +-- index.scss
        |   +-- _global.scss
        |   +-- _hero.scss
        |   +-- _variables.scss
    |   +-- index.html
    |   +-- index.js
|-- webpack
    |   +-- webpack.common.js
    |   +-- webpack.dev.js
    |   +-- webpack.prod.js
|   +-- .browserslistrc
|   +-- .eslintignore
|   +-- .eslintrc.json
|   +-- .gitignore
|   +-- .prettierignore
|   +-- .prettierrc
|   +-- babel.config.js
|   +-- jest.config.js
|   +-- LICENSE
|   +-- output.md
|   +-- package.json
|   +-- pnpm-lock.yaml
|   +-- postcss.config.js
|   +-- README.md
```

Do not worry if this looks overwhelming. We will be taking a look at what all files do and how're they arranged. So let's kickstart. You can also visit the **[github-repo](https://github.com/mandy8055/webpack-react-boilerplate)** link for getting an insight about the same.

## Basic Building blocks

Before diving deeper into individual details, let me quickly list out the technologies and tools that is used to prepare the environment:

1. **[pnpm](https://pnpm.io/)**: Fast, efficient and strict package manager.
2. **[Jest](https://jestjs.io/)** and **[React-testing-library](https://testing-library.com/docs/react-testing-library/intro/)**: For writing unit tests.
3. [webpack](https://webpack.js.org/): as a module bundler.
4. **[Babel](https://babeljs.io/)**: As a javascript compiler.
5. **[Husky](https://typicode.github.io/husky/)**: A tool that helps us run scripts before Git commits.

## Webpack configurations

Let's talk around the configurations added in webpack files one by one.

### webpack.common.js

```js
const path = require('path'); // import the path module to handle file paths.
// import Webpack plugins that will be used in the configuration.
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const ESLintWebpackPlugin = require('eslint-webpack-plugin');

const plugins = [
  new CleanWebpackPlugin(), // Cleans the output directory before each build.
  new MiniCssExtractPlugin({
    filename: '[contenthash].[name].css',
  }), // Extracts CSS into separate files.
  new ESLintWebpackPlugin(), // Runs ESLint on your code during the build process.
  new HtmlWebpackPlugin({
    template: './src/index.html',
  }), // Generates an HTML file with the bundled JavaScript and CSS files included.
];

module.exports = {
  entry: './src/index.js',
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader', // JavaScript and JSX files using Babel loader.
          options: {
            cacheDirectory: true,
          },
        },
      },
      {
        test: /\.s?[ac]?ss$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
            // This is required for asset imports in CSS, such as url()
            options: { publicPath: '' },
          },
          'css-loader',
          'postcss-loader',
          'sass-loader',
        ],
      }, // define rules for CSS and SCSS files using MiniCssExtractPlugin, CSS loader, PostCSS loader, and SASS loader.
      {
        test: /\.(png|jpe?g|gif|svg)$/i,
        type: 'asset', // Define rules for image files using the "asset" type to automatically choose between inlining images and outputting them as separate files.
      },
    ],
  },
  resolve: {
    extensions: ['.js', '.jsx'],
  },
  plugins,
  output: {
    filename: '[contenthash].bundle.js',
    path: path.resolve(__dirname, '..', 'dist'),
    assetModuleFilename: 'images/[hash][ext][query]',
  },
};
```

### webpack.dev.js

```js
const { merge } = require('webpack-merge'); // used to merge multiple Webpack configurations into a single configuration.
const path = require('path');
const commonConfiguration = require('./webpack.common.js');
const ReactRefreshWebpackPlugin = require('@pmmmwh/react-refresh-webpack-plugin');

module.exports = merge(commonConfiguration, {
  mode: 'development',
  devtool: 'source-map', // sets the devtool option to "source-map", which generates source maps for easier debugging.
  target: 'web', // sets the target option to "web", which tells Webpack to target a browser environment.
  plugins: [new ReactRefreshWebpackPlugin()],
  devServer: {
    static: path.join(__dirname, '..', 'dist'), // This sets the static option to the path of the directory where the static files are located. In this case, it uses the path.join method to join the current directory (__dirname) with the dist directory.
    compress: true, // enables gzip compression for the server responses
    port: 9000,
    hot: true, // This enables HMR for the development server, allowing changes to be applied without a full page refresh.
  },
});
```

### webpack.prod.js

```js
const { merge } = require('webpack-merge');
const commonConfiguration = require('./webpack.common.js');

module.exports = merge(commonConfiguration, {
  mode: 'production',
  target: 'browserslist', // sets the target option to "browserslist", which tells Webpack to target the browsers specified in the .browserslistrc file.
  performance: {
    maxAssetSize: 500000, // sets the maximum size (in bytes) for individual assets. Any assets that exceed this size will trigger a warning.
    maxEntrypointSize: 500000, // sets the maximum size (in bytes) for entry points (i.e., the main JavaScript file). Any entry points that exceed this size will trigger a warning.
    hints: 'error', // sets the severity level for performance warnings. In this case, it's set to "error", which will cause warnings to be treated as errors and fail the build.
    assetFilter: function (assetFilename) {
      // This function is used to filter which assets are included in the size checks. In this case, it excludes any .png files from the size limits.
      return !assetFilename.endsWith('.png');
    },
  },
});
```

In the second and final part of this blog we would be shedding light on remaining files which constitutes the setup.

{% if page.comments %} {% include disqus.md url=page.url id=page.id %} {% endif %}
