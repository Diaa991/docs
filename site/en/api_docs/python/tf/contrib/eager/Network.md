

page_type: reference
<style>{% include "site-assets/css/style.css" %}</style>


<!-- DO NOT EDIT! Automatically generated file. -->

# tf.contrib.eager.Network

## Class `Network`

Inherits From: [`Layer`](../../../tf/layers/Layer)



Defined in [`tensorflow/contrib/eager/python/network.py`](https://www.github.com/tensorflow/tensorflow/blob/r1.8/tensorflow/contrib/eager/python/network.py).

Represents the composition of a set of Layers.

`Network` implements the `Layer` interface and adds convenience methods for
managing sub-`Layer`s, such as listing variables.

`Layer`s (including other `Network`s) should be added via `track_layer`. They
can then be used when overriding the `Network.call` method:

```python
class TwoLayerNetwork(tfe.Network):

  def __init__(self, name):
    super(TwoLayerNetwork, self).__init__(name=name)
    self.layer_one = self.track_layer(tf.layers.Dense(16, input_shape=(8,)))
    self.layer_two = self.track_layer(tf.layers.Dense(1, input_shape=(16,)))

  def call(self, inputs):
    return self.layer_two(self.layer_one(inputs))
```

After constructing an object and calling the `Network`, a list of variables
created by tracked `Layer`s is available via `Network.variables`:

```python
net = TwoLayerNetwork(name="net")
output = net(tf.ones([1, 8]))
print([v.name for v in net.variables])
```

This example prints variable names, one kernel and one bias per
<a href="../../../tf/layers/Dense"><code>tf.layers.Dense</code></a> layer:

```
['net/dense/kernel:0',
 'net/dense/bias:0',
 'net/dense_1/kernel:0',
 'net/dense_1/bias:0']
```

These variables can be passed to a `Saver` (<a href="../../../tf/train/Saver"><code>tf.train.Saver</code></a>, or
<a href="../../../tf/contrib/eager/Saver"><code>tf.contrib.eager.Saver</code></a> when executing eagerly) to save or restore the
`Network`, typically alongside a global step and <a href="../../../tf/train/Optimizer"><code>tf.train.Optimizer</code></a>
variables when checkpointing during training.

Note that the semantics of calling a `Network` with graph execution (i.e. not
executing eagerly) may change slightly in the future. Currently stateful ops
are pruned from the graph unless they or something that depends on them is
executed in a session, but this behavior is not consistent with eager
execution (where stateful ops are executed eagerly). `Layer`s from <a href="../../../tf/layers"><code>tf.layers</code></a>
do not depend on this pruning and so will not be affected, but `Network`s
which rely on stateful ops being added to the graph but not executed (e.g. via
custom `Layer`s which manage stateful ops) may break with this change.

## Properties

<h3 id="activity_regularizer"><code>activity_regularizer</code></h3>

Optional regularizer function for the output of this layer.

<h3 id="dtype"><code>dtype</code></h3>



<h3 id="graph"><code>graph</code></h3>



<h3 id="inbound_nodes"><code>inbound_nodes</code></h3>

Deprecated, do NOT use! Only for compatibility with external Keras.

<h3 id="input"><code>input</code></h3>

Retrieves the input tensor(s) of a layer.

Only applicable if the layer has exactly one input,
i.e. if it is connected to one incoming layer.

#### Returns:

Input tensor or list of input tensors.


#### Raises:

* <b>`AttributeError`</b>: if the layer is connected to
    more than one incoming layers.


#### Raises:

* <b>`RuntimeError`</b>: If called in Eager mode.
* <b>`AttributeError`</b>: If no inbound nodes are found.

<h3 id="input_shape"><code>input_shape</code></h3>

Retrieves the input shape(s) of a layer.

Only applicable if the layer has exactly one input,
i.e. if it is connected to one incoming layer, or if all inputs
have the same shape.

#### Returns:

Input shape, as an integer shape tuple
(or list of shape tuples, one tuple per input tensor).


#### Raises:

* <b>`AttributeError`</b>: if the layer has no defined input_shape.
* <b>`RuntimeError`</b>: if called in Eager mode.

<h3 id="layers"><code>layers</code></h3>



<h3 id="losses"><code>losses</code></h3>

Gather losses from `Layer`s in the `Network`.

Note that when executing eagerly, `Layer.losses` evaluates
regularizers. When using graph execution, variable regularization ops have
already been created and are simply returned here.

#### Returns:

A list of tensors.

<h3 id="name"><code>name</code></h3>



<h3 id="non_trainable_variables"><code>non_trainable_variables</code></h3>



<h3 id="non_trainable_weights"><code>non_trainable_weights</code></h3>



<h3 id="outbound_nodes"><code>outbound_nodes</code></h3>

Deprecated, do NOT use! Only for compatibility with external Keras.

<h3 id="output"><code>output</code></h3>

Retrieves the output tensor(s) of a layer.

Only applicable if the layer has exactly one output,
i.e. if it is connected to one incoming layer.

#### Returns:

Output tensor or list of output tensors.


#### Raises:

* <b>`AttributeError`</b>: if the layer is connected to more than one incoming
    layers.
* <b>`RuntimeError`</b>: if called in Eager mode.

<h3 id="output_shape"><code>output_shape</code></h3>

Retrieves the output shape(s) of a layer.

Only applicable if the layer has one output,
or if all outputs have the same shape.

#### Returns:

Output shape, as an integer shape tuple
(or list of shape tuples, one tuple per output tensor).


#### Raises:

* <b>`AttributeError`</b>: if the layer has no defined output shape.
* <b>`RuntimeError`</b>: if called in Eager mode.

<h3 id="scope_name"><code>scope_name</code></h3>



<h3 id="trainable"><code>trainable</code></h3>



<h3 id="trainable_variables"><code>trainable_variables</code></h3>



<h3 id="trainable_weights"><code>trainable_weights</code></h3>



<h3 id="updates"><code>updates</code></h3>



<h3 id="variables"><code>variables</code></h3>

Returns the list of all layer variables/weights.

#### Returns:

A list of variables.

<h3 id="weights"><code>weights</code></h3>





## Methods

<h3 id="__init__"><code>__init__</code></h3>

``` python
__init__(name=None)
```

Configure the `Network`.

#### Args:

* <b>`name`</b>: The name to use for this `Network`. If specified, it must be unique
    in the context where this `Network` is first
     (1) added to another `Network` (in which case it must not share a name
       with other `Layers` added to that `Network`), or
     (2) built/called (in which case no other 'top-level' `Network`s may
      share this name).
    If unspecified or None, the `Network` will be named using its class
    name, with a number appended if necessary for uniqueness (e.g. MyNetwork
    -> 'my_network_1').


#### Raises:

* <b>`ValueError`</b>: If `name` is not valid. Note that some naming errors will
    instead be raised when the `Network` is called.

<h3 id="__call__"><code>__call__</code></h3>

``` python
__call__(
    inputs,
    *args,
    **kwargs
)
```

Wraps `call`, applying pre- and post-processing steps.

#### Arguments:

* <b>`inputs`</b>: input tensor(s).
* <b>`*args`</b>: additional positional arguments to be passed to `self.call`.
* <b>`**kwargs`</b>: additional keyword arguments to be passed to `self.call`.
    **Note**: kwarg `scope` is reserved for use by the layer.


#### Returns:

  Output tensor(s).

Note:
  - If the layer's `call` method takes a `scope` keyword argument,
    this argument will be automatically set to the current variable scope.
  - If the layer's `call` method takes a `mask` argument (as some Keras
    layers do), its default value will be set to the mask generated
    for `inputs` by the previous layer (if `input` did come from
    a layer that generated a corresponding mask, i.e. if it came from
    a Keras layer with masking support.


#### Raises:

* <b>`ValueError`</b>: if the layer's `call` method returns None (an invalid value).

<h3 id="__deepcopy__"><code>__deepcopy__</code></h3>

``` python
__deepcopy__(memo)
```



<h3 id="add_loss"><code>add_loss</code></h3>

``` python
add_loss(
    losses,
    inputs=None
)
```



<h3 id="add_update"><code>add_update</code></h3>

``` python
add_update(
    updates,
    inputs=None
)
```

Add update op(s), potentially dependent on layer inputs.

Weight updates (for instance, the updates of the moving mean and variance
in a BatchNormalization layer) may be dependent on the inputs passed
when calling a layer. Hence, when reusing the same layer on
different inputs `a` and `b`, some entries in `layer.updates` may be
dependent on `a` and some on `b`. This method automatically keeps track
of dependencies.

The `get_updates_for` method allows to retrieve the updates relevant to a
specific set of inputs.

This call is ignored in Eager mode.

#### Arguments:

* <b>`updates`</b>: Update op, or list/tuple of update ops.
* <b>`inputs`</b>: If anything other than None is passed, it signals the updates
    are conditional on some of the layer's inputs,
    and thus they should only be run where these inputs are available.
    This is the case for BatchNormalization updates, for instance.
    If None, the updates will be taken into account unconditionally,
    and you are responsible for making sure that any dependency they might
    have is available at runtime.
    A step counter might fall into this category.

<h3 id="add_variable"><code>add_variable</code></h3>

``` python
add_variable(
    name,
    shape,
    dtype=None,
    initializer=None,
    regularizer=None,
    trainable=True,
    constraint=None
)
```



<h3 id="apply"><code>apply</code></h3>

``` python
apply(
    inputs,
    *args,
    **kwargs
)
```

Apply the layer on a input.

This simply wraps `self.__call__`.

#### Arguments:

* <b>`inputs`</b>: Input tensor(s).
* <b>`*args`</b>: additional positional arguments to be passed to `self.call`.
* <b>`**kwargs`</b>: additional keyword arguments to be passed to `self.call`.


#### Returns:

Output tensor(s).

<h3 id="build"><code>build</code></h3>

``` python
build(_)
```

Creates the variables of the layer.

<h3 id="call"><code>call</code></h3>

``` python
call(
    inputs,
    **kwargs
)
```

The logic of the layer lives here.

#### Arguments:

* <b>`inputs`</b>: input tensor(s).
* <b>`**kwargs`</b>: additional keyword arguments.


#### Returns:

Output tensor(s).

<h3 id="compute_output_shape"><code>compute_output_shape</code></h3>

``` python
compute_output_shape(input_shape)
```

Computes the output shape of the layer given the input shape.

#### Args:

* <b>`input_shape`</b>: A (possibly nested tuple of) `TensorShape`.  It need not
    be fully defined (e.g. the batch size may be unknown).


#### Returns:

A (possibly nested tuple of) `TensorShape`.


#### Raises:

* <b>`TypeError`</b>: if `input_shape` is not a (possibly nested tuple of)
    `TensorShape`.
* <b>`ValueError`</b>: if `input_shape` is incomplete or is incompatible with the
    the layer.

<h3 id="count_params"><code>count_params</code></h3>

``` python
count_params()
```

Count the total number of scalars composing the weights.

#### Returns:

An integer count.


#### Raises:

* <b>`ValueError`</b>: if the layer isn't yet built
      (in which case its weights aren't yet defined).

<h3 id="get_input_at"><code>get_input_at</code></h3>

``` python
get_input_at(node_index)
```

Retrieves the input tensor(s) of a layer at a given node.

#### Arguments:

* <b>`node_index`</b>: Integer, index of the node
        from which to retrieve the attribute.
        E.g. `node_index=0` will correspond to the
        first time the layer was called.


#### Returns:

A tensor (or list of tensors if the layer has multiple inputs).


#### Raises:

* <b>`RuntimeError`</b>: If called in Eager mode.

<h3 id="get_input_shape_at"><code>get_input_shape_at</code></h3>

``` python
get_input_shape_at(node_index)
```

Retrieves the input shape(s) of a layer at a given node.

#### Arguments:

* <b>`node_index`</b>: Integer, index of the node
        from which to retrieve the attribute.
        E.g. `node_index=0` will correspond to the
        first time the layer was called.


#### Returns:

A shape tuple
(or list of shape tuples if the layer has multiple inputs).


#### Raises:

* <b>`RuntimeError`</b>: If called in Eager mode.

<h3 id="get_layer"><code>get_layer</code></h3>

``` python
get_layer(
    name=None,
    index=None
)
```

Get a contained <a href="../../../tf/layers/Layer"><code>tf.layers.Layer</code></a> either by name or index.

#### Args:

* <b>`name`</b>: String matching one of the names of a contained `Layer`. Note that
    the names of `Layer`s added to `Network`s may not be unique when doing
    layer sharing (i.e. adding a `Layer` to this `Network` which was already
    added to another `Network`). The lowest index `Layer` with a matching
    name will be returned.
* <b>`index`</b>: Integer in [0, number of layers). Layers are assigned an index
    by the order they are added.


#### Returns:

A <a href="../../../tf/layers/Layer"><code>tf.layers.Layer</code></a> object.


#### Raises:

* <b>`ValueError`</b>: If neither or both of 'index' or 'name' is specified, or the
    lookup failed.

<h3 id="get_losses_for"><code>get_losses_for</code></h3>

``` python
get_losses_for(inputs)
```

Retrieves losses relevant to a specific set of inputs.

#### Arguments:

* <b>`inputs`</b>: Input tensor or list/tuple of input tensors.


#### Returns:

List of loss tensors of the layer that depend on `inputs`.


#### Raises:

* <b>`RuntimeError`</b>: If called in Eager mode.

<h3 id="get_output_at"><code>get_output_at</code></h3>

``` python
get_output_at(node_index)
```

Retrieves the output tensor(s) of a layer at a given node.

#### Arguments:

* <b>`node_index`</b>: Integer, index of the node
        from which to retrieve the attribute.
        E.g. `node_index=0` will correspond to the
        first time the layer was called.


#### Returns:

A tensor (or list of tensors if the layer has multiple outputs).


#### Raises:

* <b>`RuntimeError`</b>: If called in Eager mode.

<h3 id="get_output_shape_at"><code>get_output_shape_at</code></h3>

``` python
get_output_shape_at(node_index)
```

Retrieves the output shape(s) of a layer at a given node.

#### Arguments:

* <b>`node_index`</b>: Integer, index of the node
        from which to retrieve the attribute.
        E.g. `node_index=0` will correspond to the
        first time the layer was called.


#### Returns:

A shape tuple
(or list of shape tuples if the layer has multiple outputs).


#### Raises:

* <b>`RuntimeError`</b>: If called in Eager mode.

<h3 id="get_updates_for"><code>get_updates_for</code></h3>

``` python
get_updates_for(inputs)
```

Retrieves updates relevant to a specific set of inputs.

#### Arguments:

* <b>`inputs`</b>: Input tensor or list/tuple of input tensors.


#### Returns:

List of update ops of the layer that depend on `inputs`.


#### Raises:

* <b>`RuntimeError`</b>: If called in Eager mode.

<h3 id="track_layer"><code>track_layer</code></h3>

``` python
track_layer(layer)
```

Track a Layer in this Network.

`Network` requires that all `Layer`s used in `call()` be tracked so that the
`Network` can export a complete list of variables.

#### Args:

* <b>`layer`</b>: A <a href="../../../tf/layers/Layer"><code>tf.layers.Layer</code></a> object.


#### Returns:

The passed in `layer`.


#### Raises:

* <b>`RuntimeError`</b>: If __init__ has not been called.
* <b>`TypeError`</b>: If `layer` is the wrong type.
* <b>`ValueError`</b>: If a `Layer` with the same name has already been added.


