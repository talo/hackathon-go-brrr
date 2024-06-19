# hackathon-go-brrr
A template repository for the inaugural QDX hackathon at the University of Melbourne!

## Objective
Make the fastest possible neural network implementation.
We will be focused only on *inference* (you do not need to train the neural network).

## Template repository description
We have provided a `weights_and_biases.txt` file, and a test dataset (under `tensors`). 
We have encoded each of our test files as a 1-D tensor with 225 elements (15x15 pixels) so you can pass them directly to the input layer of your neural network. The filename of each input tensor is the label of the underlying image.
You should perform inference on the tensors directly.

As a convenience, the underlying images are saved in `bitmaps`. The test dataset is a collection of bitmaps of letters.
To read more about bitmaps, see [here](https://en.wikipedia.org/wiki/Bitmap).

The weights (and biases) file encodes a neural network that has been trained to classify these letters with an accuracy of 100%.

You only need to implement the forward ("inference") pass of the neural network. But you need to implement it very peformantly!

## Example PyTorch implementation
We have provided an example PyTorch implementation (under `example`) so you can see how the weights can be loaded, and inference performed on the test dataset. It is assuredly not an ideal implementation, and we expect you to produce better implementations!

## Background on neural networks

A neural network consists of multiple layers, including an input layer, several hidden layers, and an output layer.

Each layer has a specific number of units (or neurons) that define the dimensions of the weight matrices and bias vectors connecting the layers.
A weight matrix connects neurons from one layer to the next.
Each entry in a weight matrix represents the strength of the connection between a neuron in the previous layer and a neuron in the next layer.
The dimensions of the weight matrix are determined by the number of neurons in the input layer and the output layer of the connection.

A bias vector is associated with each layer of neurons except the input layer.
Each entry in a bias vector corresponds to a single neuron in the layer, representing the bias added to the weighted sum of inputs for that neuron.

## How is output for each layer calculated?
For a given layer $l$, the output $\mathbf{h}^{(l+1)}$ is computed as:

$\mathbf{h}^{(l+1)} = f(\mathbf{W}^{(l+1)} \mathbf{h}^{(l)} + \mathbf{b}^{(l+1)})$

Where:

- $\mathbf{W}^{(l+1)}$ is the weight matrix for layer $l + 1$ (the next layer).
- $\mathbf{h}^{(l)}$ is the output (or activation) from the previous layer (or the input data if it’s the first layer).
- $\mathbf{b}^{(l + 1)}$ is the bias vector for layer $l + 1$ (the next layer).
- $f$ is the activation function (e.g., ReLU).

## Your task
1. Read the `weights_and_biases.txt` file. For more details on the structure of the input file, please see below.

2. Perform inference to classify each of the tensors (corresponding to a letter) within the `tensors` directory.

3. Write the results to the `results.csv` file, which should be structued like so. 
You can use this lookup table to output the guesses from your neural net:
The output classes are Aa-Zz, so A-Z and a-z interleaved. So, for example, the first four classes (in order) are A, a, B and B. You can see this in the lookup table we have provided below.
The output layer of the neural network is a 52 element 1-D tensor. Each element within the tensor is the probability of the input image being
*that* output class. To get the index for the lookup table you need to get the `argmax`, which is the index of the element with the highest probability within your output tensor.
    
    | Index | Letter | Index | Letter | Index | Letter | Index | Letter | Index | Letter | Index | Letter | Index | Letter |
    |-------|--------|-------|--------|-------|--------|-------|--------|-------|--------|-------|--------|-------|--------|
    | 0     | A      | 1     | a      | 2     | B      | 3     | b      | 4     | C      | 5     | c      | 6     | D      |
    | 7     | d      | 8     | E      | 9     | e      | 10    | F      | 11    | f      | 12    | G      | 13    | g      |
    | 14    | H      | 15    | h      | 16    | I      | 17    | i      | 18    | J      | 19    | j      | 20    | K      |
    | 21    | k      | 22    | L      | 23    | l      | 24    | M      | 25    | m      | 26    | N      | 27    | n      |
    | 28    | O      | 29    | o      | 30    | P      | 31    | p      | 32    | Q      | 33    | q      | 34    | R      |
    | 35    | r      | 36    | S      | 37    | s      | 38    | T      | 39    | t      | 40    | U      | 41    | u      |
    | 42    | V      | 43    | v      | 44    | W      | 45    | w      | 46    | X      | 47    | x      | 48    | Y      |
    | 49    | y      | 50    | Z      | 51    | z      |

    This is how we expect your csv to be structured:
    ```csv
    image_number, guess
    1, A
    2, B
    3, F
    4, J
    ```

4. As this is a small dataset and a small neural network, we would like you to run the inference 1000 times and write each prediction to a file named `predictions_{iteration_number}.csv`. We will be verifying that your code performs the inference each time, so please don't cheat!


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
For the output
 layer, `softmax` is the activation function.

## Diagram
![Sample Image](./neural_network_diagram.png)


## Format of the weights.csv file

Each section begins with a header indicating the type and layer index, structured like
so:
```
fc1.weight:
-0,2434, -0.3432, 0.3456, 0.9999, ...
fc1.bias:
0,2434, -0.3432, 0.3456, 0.9999, ...
...
```
The layers are named fc1 to fc7 for each of the layers of the neural network.

A newline seperates the header from the weights themselves. The actual weight or bias values are listed in a single row seperated by commas. Weight matrices are provided in row-major order, and biases are single dimensional rows.

