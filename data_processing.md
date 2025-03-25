---
title: Data Processing with TensorFlow.js
---

# Data Processing with TensorFlow.js

TensorFlow.js provides a powerful data API (`tfjs-data`) for efficient data loading, processing, and augmentation in your machine learning applications. This guide will walk you through the key concepts and usage of the `tfjs-data` API.

## Table of Contents

1. [Introduction](#introduction)
2. [Key Components](#key-components)
3. [Creating Datasets](#creating-datasets)
4. [Data Processing Operations](#data-processing-operations)
5. [Working with CSV Data](#working-with-csv-data)
6. [Handling Text Data](#handling-text-data)
7. [Using Generators and Functions](#using-generators-and-functions)
8. [Microphone and Webcam Input](#microphone-and-webcam-input)
9. [File and URL Data Sources](#file-and-url-data-sources)

## Introduction

The `tfjs-data` API provides a set of tools to create, transform, and manage datasets for your TensorFlow.js models. It allows you to efficiently load and preprocess data, making it easier to work with various data sources and formats.

## Key Components

The main components of the `tfjs-data` API include:

- `Dataset`: The core class for creating and manipulating datasets.
- `CSVDataset`: A specialized dataset for working with CSV data.
- `TextLineDataset`: A dataset for processing text data line by line.
- Various data readers and sources for different input types.

## Creating Datasets

To get started with data processing, you first need to create a dataset. Here's a basic example:

```javascript
import * as tf from '@tensorflow/tfjs';
import * as tfdata from '@tensorflow/tfjs-data';

// Create a simple dataset from an array
const dataset = tfdata.array([1, 2, 3, 4, 5]);
```

You can also create datasets from other sources, which we'll cover in later sections.

## Data Processing Operations

Once you have a dataset, you can perform various operations on it:

```javascript
const processedDataset = dataset
  .map(x => x * 2)  // Double each element
  .filter(x => x > 5)  // Keep only elements greater than 5
  .batch(2);  // Group elements into batches of 2

// Iterate over the processed dataset
await processedDataset.forEachAsync(batch => console.log(batch));
```

## Working with CSV Data

The `CSVDataset` class makes it easy to work with CSV files:

```javascript
import {CSVDataset} from '@tensorflow/tfjs-data';

const csvDataset = CSVDataset.create('path/to/your/file.csv', {
  columnConfigs: {
    column1: {isLabel: false},
    column2: {isLabel: true}
  }
});

// Process the CSV data
csvDataset.map(row => {
  // Perform operations on each row
  return {x: row.column1, y: row.column2};
}).batch(32);
```

## Handling Text Data

For text-based data, you can use the `TextLineDataset`:

```javascript
import {TextLineDataset} from '@tensorflow/tfjs-data';

const textDataset = TextLineDataset.create('path/to/your/textfile.txt');

textDataset.map(line => {
  // Process each line of text
  return someProcessingFunction(line);
});
```

## Using Generators and Functions

You can create datasets from JavaScript generators or functions:

```javascript
const generatorDataset = tfdata.generator(function* () {
  for (let i = 0; i < 100; i++) {
    yield i;
  }
});

const funcDataset = tfdata.func(() => Math.random());
```

## Microphone and Webcam Input

For real-time audio and video input, use the `microphone` and `webcam` functions:

```javascript
const microphoneDataset = await tfdata.microphone({
  sampleRateHz: 44100,
  numChannels: 1
});

const webcamDataset = await tfdata.webcam({
  resizeWidth: 224,
  resizeHeight: 224
});
```

## File and URL Data Sources

You can also create datasets from files or URLs:

```javascript
import {FileDataSource, URLDataSource} from '@tensorflow/tfjs-data';

const fileDataset = new FileDataSource('path/to/your/file');
const urlDataset = new URLDataSource('https://example.com/data.json');
```

These data sources can be used with other dataset types to load and process data from various locations.

By leveraging the `tfjs-data` API, you can efficiently handle data preprocessing and augmentation tasks in your TensorFlow.js applications, streamlining your machine learning workflows.