# Transfer Learning with TensorFlow.js

Transfer learning is a powerful technique that allows you to leverage pre-trained models and adapt them for your specific tasks. This guide will walk you through the process of performing transfer learning with TensorFlow.js, including how to load pre-trained models and fine-tune them for custom tasks.

## Loading Pre-trained Models

TensorFlow.js provides a convenient way to load pre-trained models using the `tf.loadLayersModel()` function. This function can load models from various sources, including local storage, IndexedDB, and HTTP servers.

```javascript
const model = await tf.loadLayersModel('https://example.com/model.json');
```

You can also load models from TensorFlow Hub by setting the `fromTFHub` option to `true`:

```javascript
const model = await tf.loadLayersModel('https://tfhub.dev/google/imagenet/mobilenet_v2_140_224/classification/2', {fromTFHub: true});
```

## Freezing Layers

When performing transfer learning, you often want to freeze the weights of certain layers in the pre-trained model. This prevents these layers from being updated during fine-tuning. You can freeze layers by setting their `trainable` property to `false`:

```javascript
// Freeze all layers except the last one
for (const layer of model.layers.slice(0, -1)) {
  layer.trainable = false;
}
```

## Adding New Layers

To adapt the pre-trained model for your specific task, you'll often need to add new layers on top of the existing ones. You can do this using the `model.add()` method for Sequential models, or by creating a new Model instance for more complex architectures:

```javascript
// For Sequential models
model.add(tf.layers.dense({units: 10, activation: 'softmax'}));

// For more complex models
const newOutput = tf.layers.dense({units: 10, activation: 'softmax'}).apply(model.outputs[0]);
const newModel = tf.model({inputs: model.inputs, outputs: newOutput});
```

## Fine-tuning the Model

Once you've prepared your model for transfer learning, you can fine-tune it on your custom dataset. This involves compiling the model with appropriate loss and optimizer functions, and then training it using the `fit()` method:

```javascript
model.compile({
  optimizer: tf.train.adam(0.0001),
  loss: 'categoricalCrossentropy',
  metrics: ['accuracy']
});

const history = await model.fit(xTrain, yTrain, {
  epochs: 10,
  validationData: [xVal, yVal],
  callbacks: tf.callbacks.earlyStopping({monitor: 'val_loss', patience: 3})
});
```

## Saving the Fine-tuned Model

After fine-tuning, you can save your model for later use:

```javascript
await model.save('localstorage://my-fine-tuned-model');
```

## Example: Transfer Learning with MobileNet

Here's a complete example demonstrating transfer learning using a pre-trained MobileNet model:

```javascript
async function performTransferLearning() {
  // Load pre-trained MobileNet model
  const mobilenet = await tf.loadLayersModel(
    'https://storage.googleapis.com/tfjs-models/tfjs/mobilenet_v1_0.25_224/model.json'
  );

  // Freeze all layers except the last one
  for (const layer of mobilenet.layers.slice(0, -1)) {
    layer.trainable = false;
  }

  // Add new layers for custom classification
  const newOutput = tf.layers.dense({units: 5, activation: 'softmax'}).apply(mobilenet.outputs[0]);
  const newModel = tf.model({inputs: mobilenet.inputs, outputs: newOutput});

  // Compile the model
  newModel.compile({
    optimizer: tf.train.adam(0.0001),
    loss: 'categoricalCrossentropy',
    metrics: ['accuracy']
  });

  // Prepare your custom dataset
  const {xTrain, yTrain, xVal, yVal} = await prepareCustomDataset();

  // Fine-tune the model
  const history = await newModel.fit(xTrain, yTrain, {
    epochs: 10,
    validationData: [xVal, yVal],
    callbacks: tf.callbacks.earlyStopping({monitor: 'val_loss', patience: 3})
  });

  // Save the fine-tuned model
  await newModel.save('localstorage://my-custom-model');

  console.log('Transfer learning complete!');
}
```

By following this guide, you should now be able to perform transfer learning with pre-trained models in TensorFlow.js, adapting them for your specific tasks and datasets.