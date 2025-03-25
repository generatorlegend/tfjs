---
title: TensorFlow.js Vis Library Guide
description: Learn how to use the TensorFlow.js Vis library for visualizing model training progress, performance metrics, and tensor data.
---

# TensorFlow.js Vis Library Guide

The TensorFlow.js Vis library provides a set of tools for visualizing model training progress, performance metrics, and tensor data. This guide will walk you through the main features of the library and how to use them in your TensorFlow.js projects.

## Table of Contents

1. [Getting Started](#getting-started)
2. [The Visor](#the-visor)
3. [Rendering Visualizations](#rendering-visualizations)
4. [Showing Model Information](#showing-model-information)
5. [Metrics and Evaluation](#metrics-and-evaluation)

## Getting Started

To use the TensorFlow.js Vis library, you need to include it in your project. You can do this by adding the following script tag to your HTML file:

```html
<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs-vis"></script>
```

Alternatively, if you're using a module bundler, you can install the package using npm:

```bash
npm install @tensorflow/tfjs-vis
```

And then import it in your JavaScript file:

```javascript
import * as tfvis from '@tensorflow/tfjs-vis';
```

## The Visor

The Visor is the main interface for displaying visualizations. It's a resizable and movable container that can be added to your web page.

To show the Visor, use the `tfvis.visor()` function:

```javascript
tfvis.visor();
```

The Visor object provides several methods for controlling its behavior:

- `open()`: Opens the Visor
- `close()`: Closes the Visor
- `toggle()`: Toggles the Visor between open and closed states
- `isOpen()`: Returns a boolean indicating if the Visor is open
- `isFullscreen()`: Returns a boolean indicating if the Visor is in fullscreen mode
- `toggleFullScreen()`: Toggles fullscreen mode for the Visor
- `setActiveTab(tabName)`: Sets the active tab in the Visor

Example usage:

```javascript
const visor = tfvis.visor();
visor.open();
visor.toggleFullScreen();
visor.setActiveTab('Visor');
```

## Rendering Visualizations

The TensorFlow.js Vis library provides several rendering functions for creating different types of visualizations. These are available under the `tfvis.render` namespace.

Some of the available visualizations include:

- `barchart`: Renders a bar chart
- `histogram`: Renders a histogram
- `linechart`: Renders a line chart
- `scatterplot`: Renders a scatter plot
- `heatmap`: Renders a heatmap
- `confusionMatrix`: Renders a confusion matrix
- `table`: Renders a table of data

Here's an example of how to create a line chart:

```javascript
const data = [
  {x: 0, y: 10},
  {x: 1, y: 5},
  {x: 2, y: 12},
  {x: 3, y: 6},
  {x: 4, y: 8},
];

const surface = tfvis.visor().surface({name: 'Line Chart', tab: 'Charts'});
tfvis.render.linechart(surface, data, {xLabel: 'X Axis', yLabel: 'Y Axis'});
```

## Showing Model Information

The `tfvis.show` namespace provides functions for displaying information about your TensorFlow.js models:

- `modelSummary`: Shows a summary of the model's layers
- `layer`: Shows information about a specific layer in the model
- `valuesDistribution`: Shows the distribution of values in a tensor

Example of showing a model summary:

```javascript
const model = tf.sequential({
  layers: [
    tf.layers.dense({inputShape: [784], units: 32, activation: 'relu'}),
    tf.layers.dense({units: 10, activation: 'softmax'}),
  ]
});

tfvis.show.modelSummary({name: 'Model Architecture'}, model);
```

## Metrics and Evaluation

The `tfvis.metrics` namespace provides functions for computing common evaluation metrics:

- `accuracy`: Computes the accuracy of predictions
- `perClassAccuracy`: Computes the per-class accuracy
- `confusionMatrix`: Computes a confusion matrix

These metrics can be used in conjunction with the rendering functions to visualize model performance.

Example of computing and visualizing accuracy:

```javascript
const trueLabels = tf.tensor1d([1, 0, 1, 1, 0]);
const predictions = tf.tensor1d([0.9, 0.1, 0.8, 0.7, 0.3]);

const accuracy = await tfvis.metrics.accuracy(trueLabels, predictions);
console.log('Accuracy:', accuracy);

const surface = tfvis.visor().surface({name: 'Model Accuracy', tab: 'Metrics'});
tfvis.render.barchart(surface, [{index: 'Accuracy', value: accuracy}]);
```

This guide covers the main features of the TensorFlow.js Vis library. For more detailed information on specific functions and their parameters, refer to the API documentation.