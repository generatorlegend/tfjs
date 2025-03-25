# Working with Models in TensorFlow.js

This guide covers how to create, train, save, and load models in TensorFlow.js. We'll explore both the Sequential API and the Functional API approaches.

## Table of Contents

1. [Creating Models](#creating-models)
   - [Sequential API](#sequential-api)
   - [Functional API](#functional-api)
2. [Training Models](#training-models)
3. [Saving and Loading Models](#saving-and-loading-models)
4. [Model Evaluation and Prediction](#model-evaluation-and-prediction)

## Creating Models

TensorFlow.js offers two main approaches for creating models: the Sequential API and the Functional API.

### Sequential API

The Sequential API is the simplest way to create models in TensorFlow.js. It's suitable for linear stack of layers where each layer has exactly one input tensor and one output tensor.

Here's an example of creating a simple Sequential model:

```javascript
const model = tf.sequential({
  layers: [
    tf.layers.dense({units: 32, inputShape: [10], activation: 'relu'}),
    tf.layers.dense({units: 16, activation: 'relu'}),
    tf.layers.dense({units: 1, activation: 'sigmoid'})
  ]
});
```

You can also add layers one by one:

```javascript
const model = tf.sequential();
model.add(tf.layers.dense({units: 32, inputShape: [10], activation: 'relu'}));
model.add(tf.layers.dense({units: 16, activation: 'relu'}));
model.add(tf.layers.dense({units: 1, activation: 'sigmoid'}));
```

### Functional API

The Functional API provides more flexibility in creating models with non-linear topology, shared layers, or multiple inputs or outputs.

Here's an example of creating a model with multiple inputs using the Functional API:

```javascript
const input1 = tf.input({shape: [10]});
const input2 = tf.input({shape: [20]});
const dense1 = tf.layers.dense({units: 4}).apply(input1);
const dense2 = tf.layers.dense({units: 8}).apply(input2);
const concat = tf.layers.concatenate().apply([dense1, dense2]);
const output = tf.layers.dense({units: 1, activation: 'sigmoid'}).apply(concat);

const model = tf.model({inputs: [input1, input2], outputs: output});
```

## Training Models

Once you've created a model, you need to compile it before training. Compiling a model involves specifying the optimizer, loss function, and optional metrics:

```javascript
model.compile({
  optimizer: 'adam',
  loss: 'binaryCrossentropy',
  metrics: ['accuracy']
});
```

After compilation, you can train the model using the `fit()` method:

```javascript
const history = await model.fit(xs, ys, {
  epochs: 50,
  batchSize: 32,
  callbacks: tf.callbacks.earlyStopping({monitor: 'val_loss'})
});
```

For larger datasets, you can use `fitDataset()` with a tf.data.Dataset:

```javascript
const dataset = tf.data.generator(dataGenerator).batch(32);
const history = await model.fitDataset(dataset, {
  epochs: 50,
  callbacks: tf.callbacks.earlyStopping({monitor: 'val_loss'})
});
```

## Saving and Loading Models

TensorFlow.js allows you to save and load models in various formats and storage options.

Saving a model:

```javascript
const saveResult = await model.save('localstorage://my-model-1');
```

Loading a model:

```javascript
const loadedModel = await tf.loadLayersModel('localstorage://my-model-1');
```

You can also save and load models to/from IndexedDB, HTTP server, or file downloads. For example:

```javascript
// Save to IndexedDB
await model.save('indexeddb://my-model-1');

// Save for file download
await model.save('downloads://my-model-1');

// Save to HTTP server
await model.save('http://my-server/model');
```

## Model Evaluation and Prediction

After training, you can evaluate your model on test data:

```javascript
const evalResult = model.evaluate(testXs, testYs);
console.log(`Test loss: ${evalResult[0].dataSync()[0]}`);
console.log(`Test accuracy: ${evalResult[1].dataSync()[0]}`);
```

To make predictions with your model:

```javascript
const predictions = model.predict(newXs);
predictions.print();
```

For classification tasks, you might want to get the class with the highest probability:

```javascript
const classPredictions = model.predict(newXs).argMax(-1);
classPredictions.print();
```

Remember to dispose of tensors when you're done with them to free up memory:

```javascript
predictions.dispose();
classPredictions.dispose();
```

This guide covers the basics of working with models in TensorFlow.js. For more advanced topics and detailed API references, please refer to the official TensorFlow.js documentation.