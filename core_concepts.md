# Core Concepts in TensorFlow.js

TensorFlow.js is a powerful library for machine learning in JavaScript. This guide will introduce you to the core concepts of TensorFlow.js, including tensors, operations, models, and layers.

## Tensors

Tensors are the fundamental data structure in TensorFlow.js. They represent multi-dimensional arrays of numbers and are the primary way to store and manipulate data.

### Creating Tensors

You can create tensors using the `tf.tensor()` function:

```javascript
const tensor1d = tf.tensor([1, 2, 3, 4]);
const tensor2d = tf.tensor([[1, 2], [3, 4]]);
```

TensorFlow.js also provides convenience functions for creating tensors of specific ranks:

```javascript
const scalar = tf.scalar(3.14);
const vector = tf.tensor1d([1, 2, 3]);
const matrix = tf.tensor2d([[1, 2], [3, 4]]);
```

### Tensor Properties

Tensors have several important properties:

- `shape`: The dimensions of the tensor
- `dtype`: The data type of the tensor (e.g., 'float32', 'int32')
- `rank`: The number of dimensions of the tensor

```javascript
const tensor = tf.tensor([[1, 2], [3, 4]]);
console.log(tensor.shape);    // [2, 2]
console.log(tensor.dtype);    // 'float32'
console.log(tensor.rank);     // 2
```

## Operations

TensorFlow.js provides a wide range of operations to manipulate and perform computations on tensors.

### Basic Operations

```javascript
const a = tf.tensor([1, 2, 3]);
const b = tf.tensor([4, 5, 6]);

const sum = a.add(b);         // Element-wise addition
const product = a.mul(b);     // Element-wise multiplication
const squared = a.square();   // Element-wise squaring
```

### Chaining Operations

You can chain operations to create more complex computations:

```javascript
const result = a.add(b).square().mean();
```

### Using `tf.tidy()`

To manage memory efficiently, use `tf.tidy()` to automatically clean up intermediate tensors:

```javascript
const result = tf.tidy(() => {
  const x = tf.tensor([1, 2, 3]);
  const y = x.square().log().neg();
  return y.mean();
});
```

## Models

Models in TensorFlow.js represent a function that maps inputs to outputs. The library provides two main ways to create models: Sequential and Functional.

### Sequential Model

A Sequential model is a linear stack of layers:

```javascript
const model = tf.sequential();
model.add(tf.layers.dense({units: 32, inputShape: [784]}));
model.add(tf.layers.dense({units: 10, activation: 'softmax'}));

model.compile({
  optimizer: 'sgd',
  loss: 'categoricalCrossentropy',
  metrics: ['accuracy']
});
```

### Functional Model

Functional models allow for more complex architectures:

```javascript
const input = tf.input({shape: [784]});
const dense1 = tf.layers.dense({units: 32}).apply(input);
const dense2 = tf.layers.dense({units: 10, activation: 'softmax'}).apply(dense1);
const model = tf.model({inputs: input, outputs: dense2});

model.compile({
  optimizer: 'adam',
  loss: 'categoricalCrossentropy',
  metrics: ['accuracy']
});
```

## Layers

Layers are the building blocks of neural networks in TensorFlow.js. They encapsulate a set of weights and a specific computation.

### Common Layer Types

- Dense (Fully Connected) Layer:
```javascript
tf.layers.dense({units: 32, activation: 'relu'});
```

- Convolutional Layer:
```javascript
tf.layers.conv2d({filters: 32, kernelSize: 3, activation: 'relu'});
```

- Pooling Layer:
```javascript
tf.layers.maxPooling2d({poolSize: 2, strides: 2});
```

- Recurrent Layer:
```javascript
tf.layers.lstm({units: 64});
```

### Custom Layers

You can create custom layers by extending the `tf.layers.Layer` class:

```javascript
class MyCustomLayer extends tf.layers.Layer {
  constructor() {
    super({});
  }

  build(inputShape) {
    this.kernel = this.addWeight(
      'kernel',
      [inputShape[1], 1],
      'float32',
      tf.initializers.randomNormal()
    );
  }

  call(inputs) {
    return tf.matMul(inputs, this.kernel);
  }

  computeOutputShape(inputShape) {
    return [inputShape[0], 1];
  }
}

const myLayer = new MyCustomLayer();
```

## Conclusion

This guide has introduced you to the core concepts of TensorFlow.js: tensors, operations, models, and layers. These building blocks form the foundation for creating and training machine learning models in JavaScript. As you continue to explore TensorFlow.js, you'll discover more advanced features and techniques for solving complex machine learning problems.