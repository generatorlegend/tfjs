---
title: Core Concepts in TensorFlow.js
---

# Core Concepts in TensorFlow.js

TensorFlow.js is a powerful library for machine learning in JavaScript. This guide will introduce you to the core concepts of TensorFlow.js, including tensors, operations, models, and layers. Understanding these concepts will help you build and deploy machine learning models in the browser or Node.js environment.

## Tensors

Tensors are the fundamental data structure in TensorFlow.js. They are multi-dimensional arrays that can represent various types of data, such as numbers, strings, or even more complex structures.

### Creating Tensors

You can create tensors using the `tf.tensor()` function:

```javascript
const tensor1d = tf.tensor([1, 2, 3, 4]);
const tensor2d = tf.tensor([[1, 2], [3, 4]]);
const tensor3d = tf.tensor([[[1], [2]], [[3], [4]]]);
```

TensorFlow.js also provides convenience functions for creating tensors with specific shapes:

```javascript
const zeros = tf.zeros([2, 3]);
const ones = tf.ones([2, 3]);
const random = tf.randomNormal([2, 3]);
```

## Operations

TensorFlow.js provides a wide range of operations that can be performed on tensors. These operations include mathematical computations, tensor manipulations, and more.

### Basic Operations

```javascript
const a = tf.tensor([1, 2, 3]);
const b = tf.tensor([4, 5, 6]);

const sum = a.add(b);
const product = a.mul(b);
const difference = a.sub(b);
```

### Chaining Operations

You can chain multiple operations together:

```javascript
const result = tf.tensor([1, 2, 3])
  .square()
  .log()
  .add(tf.scalar(1));
```

## Models

In TensorFlow.js, models are used to define the structure and behavior of machine learning algorithms. There are two main types of models: `tf.Sequential` and `tf.Model`.

### Sequential Model

A `tf.Sequential` model is a linear stack of layers. It's the simplest and most common type of model.

```javascript
const model = tf.sequential();
model.add(tf.layers.dense({ units: 32, inputShape: [50] }));
model.add(tf.layers.dense({ units: 4 }));

console.log(JSON.stringify(model.outputs[0].shape));
```

### Functional API Model

The Functional API allows you to create more complex model architectures with multiple inputs and outputs.

```javascript
const input = tf.input({ shape: [5] });
const denseLayer1 = tf.layers.dense({ units: 10, activation: 'relu' });
const denseLayer2 = tf.layers.dense({ units: 4, activation: 'softmax' });

const output = denseLayer2.apply(denseLayer1.apply(input));
const model = tf.model({ inputs: input, outputs: output });

model.predict(tf.ones([2, 5])).print();
```

## Layers

Layers are the building blocks of neural networks in TensorFlow.js. They encapsulate sets of operations and weights that can be reused across different parts of a model.

### Common Layer Types

1. Dense (Fully Connected) Layer:
```javascript
tf.layers.dense({ units: 32, activation: 'relu' });
```

2. Convolutional Layer:
```javascript
tf.layers.conv2d({ filters: 32, kernelSize: 3, activation: 'relu' });
```

3. Recurrent Layer:
```javascript
tf.layers.lstm({ units: 64 });
```

### Creating Custom Layers

You can create custom layers by extending the `tf.layers.Layer` class:

```javascript
class MyCustomLayer extends tf.layers.Layer {
  constructor(config) {
    super(config);
  }

  build(inputShape) {
    // Create the layer's variables
  }

  call(inputs) {
    // Define the layer's operations
  }

  computeOutputShape(inputShape) {
    // Specify the output shape
  }
}
```

## Conclusion

These core concepts form the foundation of working with TensorFlow.js. By understanding tensors, operations, models, and layers, you'll be well-equipped to build and train machine learning models in JavaScript. As you progress, you can explore more advanced topics such as custom training loops, model optimization, and deployment strategies.

For more detailed information and advanced usage, please refer to the official TensorFlow.js documentation and API reference.