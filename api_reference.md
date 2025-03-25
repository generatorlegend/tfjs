---
title: TensorFlow.js API Reference
---

# TensorFlow.js API Reference

This document provides a comprehensive API reference for TensorFlow.js, covering all public methods, classes, and functions across the various packages (core, layers, data, converter, vis).

## Table of Contents

1. [Core Package](#core-package)
2. [Layers Package](#layers-package)
3. [Data Package](#data-package)
4. [Converter Package](#converter-package)
5. [Visualization Package](#visualization-package)

## Core Package

The core package provides the fundamental building blocks for TensorFlow.js.

### Optimizers

```typescript
import {registerOptimizers} from './optimizers/register_optimizers';
registerOptimizers();
```

The core package includes various optimizers for training models. These are registered using the `registerOptimizers` function.

### Exports

The core package exports all its functionality from the `base` module:

```typescript
export * from './base';
```

## Layers Package

The layers package provides high-level APIs for building neural networks.

### Main Exports

```typescript
import * as constraints from './exports_constraints';
import * as initializers from './exports_initializers';
import * as layers from './exports_layers';
import * as metrics from './exports_metrics';
import * as models from './exports_models';
import * as regularizers from './exports_regularizers';
```

These modules provide various components for building and configuring neural network layers.

### Key Classes and Interfaces

- `CallbackList`, `CustomCallback`, `CustomCallbackArgs`, `History`
- `Callback`, `EarlyStopping`, `EarlyStoppingCallbackArgs`
- `InputSpec`, `SymbolicTensor`
- `LayersModel`, `ModelCompileArgs`, `ModelEvaluateArgs`
- `ModelFitDatasetArgs`, `ModelFitArgs`
- `ClassWeight`, `ClassWeightMap`
- `Shape`
- `GRUCellLayerArgs`, `GRULayerArgs`, `LSTMCellLayerArgs`, `LSTMLayerArgs`, `RNN`, `RNNLayerArgs`, `SimpleRNNCellLayerArgs`, `SimpleRNNLayerArgs`
- `Logs`
- `ModelAndWeightsConfig`, `Sequential`, `SequentialArgs`
- `LayerVariable`

### Key Functions

- `input`
- `loadLayersModel`
- `model`
- `registerCallbackConstructor`
- `sequential`

## Data Package

The data package provides utilities for working with datasets and data loading.

### Key Classes

- `Dataset`
- `CSVDataset`
- `TextLineDataset`
- `FileDataSource`
- `URLDataSource`

### Key Functions

- `array`
- `zip`
- `csv`
- `func`
- `generator`
- `microphone`
- `webcam`

### Interfaces

- `ColumnConfig`
- `MicrophoneConfig`
- `WebcamConfig`

## Converter Package

The converter package provides utilities for loading and converting models from other formats.

### Key Classes and Interfaces

- `GraphModel`
- `IAttrValue`
- `INameAttrList`
- `INodeDef`
- `ITensor`
- `ITensorShape`
- `GraphNode`
- `OpExecutor`

### Key Functions

- `loadGraphModel`
- `loadGraphModelSync`
- `deregisterOp`
- `registerOp`

## Visualization Package

The visualization package provides tools for visualizing model training and performance.

### Render Functions

- `barchart`
- `table`
- `histogram`
- `linechart`
- `scatterplot`
- `confusionMatrix`
- `heatmap`

### Metrics Functions

- `accuracy`
- `perClassAccuracy`
- `confusionMatrix`

### Show Functions

- `history`
- `fitCallbacks`
- `perClassAccuracy`
- `valuesDistribution`
- `layer`
- `modelSummary`

### Other Exports

- `visor`

## Version Information

Each package provides a version constant:

- Core: `version`
- Layers: `version_layers`
- Data: `version_data`
- Converter: `version_converter`
- Visualization: `version_vis`

For detailed information on specific functions, classes, or methods, please refer to the inline documentation in the respective source files.