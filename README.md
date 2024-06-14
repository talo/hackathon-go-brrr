# hackathon-go-brrr
A template repository for the inaugural QDX hackathon at the University of Melbourne!

## Objective

Make the fastest possible neural network implementation.
We will be focused only on *inference* (you do not need to train the neural network).
We have provided a weights.csv file, and a test dataset (under `data_set`). The test dataset is a collection of bitmaps of letters.
To read more about bitmaps, see [here](https://en.wikipedia.org/wiki/Bitmap).

The weights (and biases) file encodes a neural network that has been trained to classify these letters with an accuracy of 100%.

The architecture is summarised below.
You only need to implement the forward ("inference") pass of the neural network.

## Your task
1. Read the `weights.csv` file.
2. Perform inference to classify each of the bitmaps within the `data_set` directory. Please note that the images themselves are provided as a convenience; your classification should be performed directly from the bitmaps.
3. Write the results to the `results.csv` file, which should be structued like so.

    ```csv
    image_number, guess
    1, A
    2, B
    3, F
    4, J
    ```
    We will be using an automatic grader for these results, so please ensure that your neural network implementation looks at it exactly.

## Background on neural networks

A neural network consists of multiple layers, including an input layer, several hidden layers, and an output layer.

Each layer has a specific number of units (or neurons) that define the dimensions of the weight matrices and bias vectors connecting the layers.
A weight matrix connects neurons from one layer to the next.

Each entry in a weight matrix represents the strength of the connection between a neuron in the previous layer and a neuron in the next layer.
The dimensions of the weight matrix are determined by the number of neurons in the input layer and the output layer of the connection.

A bias vector is associated with each layer of neurons except the input layer.
Each entry in a bias vector corresponds to a single neuron in the layer, representing the bias added to the weighted sum of inputs for that neuron.

## Neural network architecture

| Layer                          | Input Units | Output Units | Weight Matrix Dimensions | Bias Vector Dimensions |
|--------------------------------|-------------|--------------|--------------------------|------------------------|
| Input -> 1st Hidden Layer    | 225         | 98           | (225, 98)                | (98,)                  |
| 1st Hidden -> 2nd Hidden  | 98          | 65           | (98, 65)                 | (65,)                  |
| 2nd Hidden -> 3rd Hidden  | 65          | 50           | (65, 50)                 | (50,)                  |
| 3rd Hidden -> 4th Hidden  | 50          | 30           | (50, 30)                 | (30,)                  |
| 4th Hidden -> 5th Hidden  | 30          | 25           | (30, 25)                 | (25,)                  |
| 5th Hidden -> 6th Hidden   | 25          | 40           | (25, 40)                 | (40,)                  |
| 6th Hidden -> Output Layer   | 40          | 52           | (40, 52)                 | (52,)                  |

Between the input and the hidden layers
, `ReLU` was the activation function used.
For the output layer, `softmax` is the activation function.

## Format of the weights.csv file

Each section begins with a header indicating the type and layer index. For example, W[0] for the weights of the first layer and b[0] for the biases of the first layer. 

After each header, there is a separator line made of dashes (-----) to separate the header from the data. The actual weight or bias values are listed in rows. Weight matrices are provided in row-major order (and each section corresponds to the dimensions of that layer), and bias vectors are provided as a single column of values.
