---
title: Deployment Guide for TensorFlow.js
description: Instructions for deploying TensorFlow.js models in various environments
---

# Deployment Guide for TensorFlow.js

This guide provides instructions for deploying TensorFlow.js models in various environments, including web browsers, Node.js, and mobile applications using React Native.

## Table of Contents

1. [Web Browser Deployment](#web-browser-deployment)
2. [Node.js Deployment](#nodejs-deployment)
3. [React Native Deployment](#react-native-deployment)

## Web Browser Deployment

To deploy a TensorFlow.js model in a web browser:

1. Include the TensorFlow.js library in your HTML file:

```html
<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>
```

2. Load your model using one of the following methods:

   a. Load a pre-trained model:

   ```javascript
   const model = await tf.loadLayersModel('path/to/model.json');
   ```

   b. Train a model in the browser:

   ```javascript
   const model = tf.sequential();
   // Add layers and compile the model
   await model.fit(xs, ys, { epochs: 100 });
   ```

3. Use the model for inference:

```javascript
const prediction = model.predict(tf.tensor2d([[5]], [1, 1]));
```

## Node.js Deployment

To deploy a TensorFlow.js model in a Node.js environment:

1. Install the required packages:

```bash
npm install @tensorflow/tfjs-node
```

2. Import TensorFlow.js in your Node.js script:

```javascript
const tf = require('@tensorflow/tfjs-node');
```

3. Load and use your model:

```javascript
async function runModel() {
  const model = await tf.loadLayersModel('file://path/to/model.json');
  const prediction = model.predict(tf.tensor2d([[5]], [1, 1]));
  prediction.print();
}

runModel();
```

## React Native Deployment

To deploy a TensorFlow.js model in a React Native application:

1. Install the required packages:

```bash
npm install @tensorflow/tfjs @tensorflow/tfjs-react-native
```

2. Set up the TensorFlow.js environment in your app:

```javascript
import * as tf from '@tensorflow/tfjs';
import { bundleResourceIO } from '@tensorflow/tfjs-react-native';

async function setupTensorFlow() {
  await tf.ready();
  // TensorFlow.js is now ready to use
}
```

3. Load your model using bundleResourceIO:

```javascript
const modelJson = require('./assets/model.json');
const modelWeights = require('./assets/model_weights.bin');

async function loadModel() {
  const model = await tf.loadLayersModel(bundleResourceIO(modelJson, modelWeights));
  return model;
}
```

4. Use the model for inference:

```javascript
async function makePrediction(model, input) {
  const inputTensor = tf.tensor2d([input]);
  const prediction = await model.predict(inputTensor);
  return prediction;
}
```

Remember to handle permissions and camera access if you're using image-based models in React Native. You can use the `camera` and `cameraStream` exports from `@tensorflow/tfjs-react-native` to work with device cameras.

For more advanced usage and optimizations, refer to the official TensorFlow.js documentation and the specific documentation for each platform.