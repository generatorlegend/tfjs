# Getting Started with TensorFlow.js

TensorFlow.js is a powerful library that allows you to build and run machine learning models in JavaScript, both in the browser and in Node.js environments. This guide will help you get started with TensorFlow.js, covering installation, basic usage, and simple examples for web browsers and Node.js.

## Installation

### For Web Browsers

To use TensorFlow.js in a web browser, you can include it via a script tag:

```html
<script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@latest"></script>
```

### For Node.js

To use TensorFlow.js in a Node.js environment, you'll need to install it using npm:

```bash
npm install @tensorflow/tfjs-node
```

For GPU support, use:

```bash
npm install @tensorflow/tfjs-node-gpu
```

## Basic Usage

### In Web Browsers

After including the TensorFlow.js script in your HTML file, you can start using it in your JavaScript code:

```javascript
// Create a simple model
const model = tf.sequential();
model.add(tf.layers.dense({units: 1, inputShape: [1]}));

// Prepare the model for training
model.compile({loss: 'meanSquaredError', optimizer: 'sgd'});

// Generate some synthetic data for training
const xs = tf.tensor2d([1, 2, 3, 4], [4, 1]);
const ys = tf.tensor2d([1, 3, 5, 7], [4, 1]);

// Train the model
model.fit(xs, ys, {epochs: 10}).then(() => {
  // Use the model to do inference on a data point
  model.predict(tf.tensor2d([5], [1, 1])).print();
});
```

### In Node.js

First, import TensorFlow.js in your Node.js script:

```javascript
const tf = require('@tensorflow/tfjs-node');

// Your TensorFlow.js code here
// For example:
const model = tf.sequential();
model.add(tf.layers.dense({units: 1, inputShape: [1]}));
model.compile({loss: 'meanSquaredError', optimizer: 'sgd'});

// ... rest of your code
```

## Simple Examples

### Example 1: Linear Regression in the Browser

Here's a simple linear regression example you can run in the browser:

```html
<!DOCTYPE html>
<html>
<head>
  <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@latest"></script>
</head>
<body>
  <script>
    async function run() {
      // Create a linear model
      const model = tf.sequential();
      model.add(tf.layers.dense({units: 1, inputShape: [1]}));

      // Prepare the model for training
      model.compile({loss: 'meanSquaredError', optimizer: 'sgd'});

      // Generate synthetic data for training
      const xs = tf.tensor2d([-1, 0, 1, 2, 3, 4], [6, 1]);
      const ys = tf.tensor2d([-3, -1, 1, 3, 5, 7], [6, 1]);

      // Train the model
      await model.fit(xs, ys, {epochs: 250});

      // Use the model to do inference on a data point
      document.body.innerHTML += '<p>When X is 5, Y is approximately ' + 
        model.predict(tf.tensor2d([5], [1, 1])).dataSync() + '</p>';
    }

    run();
  </script>
</body>
</html>
```

### Example 2: Image Classification in Node.js

Here's a simple example of how to use a pre-trained MobileNet model for image classification in Node.js:

```javascript
const tf = require('@tensorflow/tfjs-node');
const mobilenet = require('@tensorflow-models/mobilenet');
const fs = require('fs');
const jpeg = require('jpeg-js');

// Load and prepare an image
const readImage = path => {
  const buf = fs.readFileSync(path);
  const pixels = jpeg.decode(buf, true);
  return tf.tensor3d(pixels.data, [pixels.height, pixels.width, 3]).div(255);
}

async function classifyImage(path) {
  const image = readImage(path);
  const mobilenetModel = await mobilenet.load();
  const predictions = await mobilenetModel.classify(image);
  console.log('Classification results:', predictions);
}

// Replace 'path/to/image.jpg' with the path to your image
classifyImage('path/to/image.jpg');
```

Note: You'll need to install additional dependencies for this example:

```bash
npm install @tensorflow-models/mobilenet jpeg-js
```

## Next Steps

This guide provides a basic introduction to TensorFlow.js. To learn more, explore the following topics:

1. Model creation and training
2. Data preprocessing and augmentation
3. Transfer learning
4. Saving and loading models
5. Advanced optimizations for browser and Node.js environments

For more detailed information and advanced usage, refer to the official TensorFlow.js documentation.