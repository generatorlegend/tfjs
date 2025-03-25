# TensorFlow.js API Reference

This document provides a comprehensive API reference for TensorFlow.js, including all public methods, classes, and interfaces across the core, layers, data, and converter packages.

## Table of Contents

1. [Core Package](#core-package)
2. [Layers Package](#layers-package)
3. [Data Package](#data-package)
4. [Converter Package](#converter-package)

## Core Package

The core package provides the fundamental operations and data structures for TensorFlow.js.

### Functions

#### `registerOptimizers()`

Registers optimizers for use in TensorFlow.js.

```typescript
import {registerOptimizers} from './optimizers/register_optimizers';
registerOptimizers();
```

## Layers Package

The layers package provides higher-level APIs for building and training neural networks.

### Functions

#### `model(args: ContainerArgs): LayersModel`

Creates a `tf.LayersModel` instance.

```javascript
const input = tf.input({shape: [5]});
const denseLayer1 = tf.layers.dense({units: 10, activation: 'relu'});
const denseLayer2 = tf.layers.dense({units: 4, activation: 'softmax'});
const output = denseLayer2.apply(denseLayer1.apply(input));
const model = tf.model({inputs: input, outputs: output});
```

#### `sequential(config?: SequentialArgs): Sequential`

Creates a `tf.Sequential` model.

```javascript
const model = tf.sequential();
model.add(tf.layers.dense({units: 32, inputShape: [50]}));
model.add(tf.layers.dense({units: 4}));
```

#### `input(config: InputConfig): SymbolicTensor`

Creates an input for a model as a `tf.SymbolicTensor`.

```javascript
const x = tf.input({shape: [32]});
const y = tf.layers.dense({units: 3, activation: 'softmax'}).apply(x);
const model = tf.model({inputs: x, outputs: y});
```

#### `loadLayersModel()`

Loads a layers model from a URL or `IOHandler`.

#### `registerCallbackConstructor(verbosityLevel: number, callbackConstructor: BaseCallbackConstructor): void`

Registers a callback constructor for use in model training.

## Data Package

The data package provides utilities for working with data in TensorFlow.js.

### Classes

#### `Dataset`

Represents a dataset for use in TensorFlow.js.

#### `CSVDataset`

A dataset for working with CSV data.

#### `TextLineDataset`

A dataset for working with text line data.

### Functions

#### `array()`

Creates a `Dataset` from an array.

#### `zip()`

Combines multiple datasets into a single dataset.

#### `csv()`

Creates a `CSVDataset` from a source.

#### `func()`

Creates a `Dataset` from a custom function.

#### `generator()`

Creates a `Dataset` from a generator function.

#### `microphone()`

Creates a `Dataset` from microphone input.

#### `webcam()`

Creates a `Dataset` from webcam input.

## Converter Package

The converter package provides utilities for converting models from other formats to TensorFlow.js.

### Classes

#### `GraphModel`

Represents a TensorFlow.js graph model.

### Functions

#### `loadGraphModel(modelUrl: string | io.IOHandler): Promise<GraphModel>`

Loads a `GraphModel` from a URL or `IOHandler`.

#### `loadGraphModelSync(modelUrl: string | io.IOHandler): GraphModel`

Synchronously loads a `GraphModel` from a URL or `IOHandler`.

#### `deregisterOp(name: string): void`

Deregisters a custom operation.

#### `registerOp(name: string, opImpl: OpExecutor): void`

Registers a custom operation.

### Interfaces

#### `IAttrValue`

Represents an attribute value in a TensorFlow graph.

#### `INameAttrList`

Represents a list of named attributes in a TensorFlow graph.

#### `INodeDef`

Represents a node definition in a TensorFlow graph.

#### `ITensor`

Represents a tensor in a TensorFlow graph.

#### `ITensorShape`

Represents the shape of a tensor in a TensorFlow graph.

#### `GraphNode`

Represents a node in a TensorFlow.js graph.

#### `OpExecutor`

Represents an operation executor in TensorFlow.js.

This API reference provides an overview of the main components and functions available in TensorFlow.js. For more detailed information on each item, please refer to the specific package documentation.