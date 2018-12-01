# pytorch-mnist-stroke

PyTorch helper ```Dataset``` class for downloading and parsing the [MNIST digits stroke sequence data](https://github.com/edwin-de-jong/mnist-digits-stroke-sequence-data) dataset.

## Usage

Just use it as you would with any other pytorch dataset

```python
loader = torch.utils.data.DataLoader(
            MNISTStroke(
                "/tmp/mnist_stroke", train=True
            ),batch_size=5, shuffle=True
        )


for batch_X, batch_Y in loader:
    print(batch_X, batch_Y)
```

Each element of batch_X will be something like:

| dx | dy | eos | eod |
|----|----|-----|-----|
| 0  | 1  |  0  |  0  |
| -1 | 1  |  0  |  0  |
| .  | .  |  .  |  .  |
| .  | .  |  .  |  .  |
| 0  | 0  |  0  |  1  |
| .  | .  |  .  |  .  |
| 0  | 0  |  0  |  0  |
| 0  | 0  |  0  |  0  |

And each element of batch_Y will be something like:

| class |
|-------|
| 0 |
| 2 |
| . |
| . |
| 7 |

## Differences from the original Dataset

The only difference is that the inputs sequences lenghts are all equal to the maximum one and the remaning spaces on the shorter ones are **zero filled**.

For example if the original dataset contains the sequence of lenght 4:

| dx | dy | eos | eod |
|----|----|-----|-----|
| 0  | 1  |  0  |  0  |
| -1 | 1  |  0  |  0  |
| 1  | 0  |  0  |  0  |
| 0  | 0  |  0  |  1  |

And the maximum one is 6, then in this dataset the corresponding sequence is 

| dx | dy | eos | eod |
|----|----|-----|-----|
| 0  | 1  |  0  |  0  |
| -1 | 1  |  0  |  0  |
| 1  | 0  |  0  |  0  |
| 0  | 0  |  0  |  1  |
| 0  | 0  |  0  |  0  |
| 0  | 0  |  0  |  0  |

This has been done in order to use the dataset in batchs


## Arguments

```python
MNISTStroke(location, train=train, transform=transform)
```

| Argument | Description | Example|
|----------|-------------|--------|
|location  | Location in which download and extract the dataset | /tmp/mnist_stroke|
|train     | True if you need the train set and False for the test set | False |
|transform | Function to apply to each input tensor | lambda x: x.view(x.numel()) |