

page_type: reference


<!-- DO NOT EDIT! Automatically generated file. -->
# tf.contrib.rnn.LSTMStateTuple

### `class tf.contrib.rnn.LSTMStateTuple`
### `class tf.contrib.rnn.core_rnn_cell.LSTMStateTuple`



Defined in [`tensorflow/contrib/rnn/python/ops/core_rnn_cell_impl.py`](https://www.github.com/tensorflow/tensorflow/blob/r1.1/tensorflow/contrib/rnn/python/ops/core_rnn_cell_impl.py).

See the guide: [RNN and Cells (contrib) > Classes storing split `RNNCell` state](../../../../../api_guides/python/contrib.rnn#Classes_storing_split_RNNCell_state)

Tuple used by LSTM Cells for `state_size`, `zero_state`, and output state.

Stores two elements: `(c, h)`, in that order.

Only used when `state_is_tuple=True`.

## Properties

<h3 id="c"><code>c</code></h3>

Alias for field number 0

<h3 id="dtype"><code>dtype</code></h3>



<h3 id="h"><code>h</code></h3>

Alias for field number 1



## Methods

<h3 id="__new__"><code>__new__</code></h3>

``` python
__new__(
    _cls,
    c,
    h
)
```

Create new instance of LSTMStateTuple(c, h)


