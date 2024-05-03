Domain Setup
================

.. code-block:: python

    class DomainSetup(NamedTuple):
        x: AxisSetup   
        y: AxisSetup
        z: AxisSetup
        decomposition: DomainDecompositionSetup    
        particle_setup: ParticleSetup    
        active_axes: Tuple[str]    
        active_axes_indices: Tuple[int]
        inactive_axes: Tuple[str]
        inactive_axes_indices: Tuple[int]
        dim: int     


Axis Setup
----------------

.. code-block:: python

    class AxisSetup(NamedTuple):
        cells: int         
        range: Tuple   
        stretching: MeshStretchingSetup  

* ``cells``: Choose the number of cells for descretization in each axis.
* ``range``: Choose the spatial range. (Tuple or List?)

Mesh Stretching Setup
^^^^^^^^^^^^^^^^^^^^^
Use this setup block to get more precision in a specific region. 

.. code-block:: python

    class MeshStretchingSetup(NamedTuple):
        type: str  
        tanh_value: float    
        ratio_fine_region: float      
        cells_fine: float     
        piecewise_parameters: Tuple[PiecewiseStretchingParameters]   

* ``type``: In JAX-Fluids, following types are provided: ("CHANNEL", "BOUNDARY_LAYER", "PIECEWISE", "BUBBLE_1", "BUBBLE_2") + "HOMOGENEOUS" (?)
* ``tanh_value``: If you choose "CHANNEL" or "BOUNDARY_LAYER", you need to define this value.
* ``ratio_fine_region``: If you choose "BUBBLE_1" or "BUBBLE_2", you need to define this value.
* ``cells_fine``: If you choose "BUBBLE_1" or "BUBBLE_2", you need to define this value.

.. note::

    For the current example, if required, we need to choose "PIECEWISE" type. 

Piecewise Stretching Parameters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
In each axis, divide the spatial range to pieces. Then, specify the count and stretching type of cells for each piece.
Strestching refers to the change of the cell size.
.. Naturally, it is decrease-constant-increase. 
.. >>> |   |  | | | | |  |   |    | :)

.. code-block:: python

    class PiecewiseStretchingParameters(NamedTuple):
        type: str 
        cells: int
        upper_bound: float
        lower_bound: float

* ``type``: Choose the stretching type as following: ("DECREASING", "INCREASING", "CONSTANT")
* ``cells``: Choose the divided number of cells for the piece.
* ``upper_bound``, ``lower_bound``: Specify the pisce range.


Domain Decomposition Setup
-------------------------
Split the domain into subdomains that can be solved in parallel and independently.
.. but require data exchange on the domain boundaries.
.. I guess it just solves the axes independant from eachother, which it seems we want it! so we add it with the default int=1.

.. code-block:: python

    class DomainDecompositionSetup(NamedTuple):
        split_x: int = 1
        split_y: int = 1
        split_z: int = 1

* ``split_x``, ``split_y``, ``split_z``:(?)

Particle Setup
--------------
Use this setup block if the problem involves particles. For this example, it can be skkiped.

.. code-block:: python

    class ParticleSetup(NamedTuple):
        particle_count: int = 0    

Other Parameters
----------------

Use the remaining parameters optionally to define the domein precisely. Axes = [x.y.z] and Indices = [0,1,2] 

* ``active_axes``
* ``active_axes_indices``
* ``inactive_axes``
* ``inactive_axes_indices``
* ``dim``: Dimension of the domain. (2D or 3D) (?)

Below, the example setup for the domain is shown.

.. code-block:: python

        "domain": {
        "x": {
            "cells": 80,
            "range": [
                -10.0,
                40.0
            ],
            "stretching": {
                "type": "PIECEWISE",
                "parameters": [
                    {
                    "type": "DECREASING",
                    "lower_bound": -10.0,
                    "upper_bound": -4.0,
                    "cells": 20
                    },
                    {
                    "type": "CONSTANT",
                    "lower_bound": -4.0,
                    "upper_bound": 4.0,
                    "cells": 40
                    },
                    {
                    "type": "INCREASING",
                    "lower_bound": 4.0,
                    "upper_bound": 40.0,
                    "cells": 20
                    }
                ]
            }
        },
        "y": {
            "cells": 80,
            "range": [
                -10.0,
                10.0
            ],
            "stretching": {
                "type": "PIECEWISE",
                "parameters": [
                    {
                    "type": "DECREASING",
                    "lower_bound": -10.0,
                    "upper_bound": -4.0,
                    "cells": 20
                    },
                    {
                    "type": "CONSTANT",
                    "lower_bound": -4.0,
                    "upper_bound": 4.0,
                    "cells": 40
                    },
                    {
                    "type": "INCREASING",
                    "lower_bound": 4.0,
                    "upper_bound": 10.0,
                    "cells": 20
                    }
                ]
            }
        },
        "z": {
            "cells": 1,
            "range": [
                0.0,
                1.0
            ]
        },
        "decomposition": {
            "split_x": 1,
            "split_y": 1,
            "split_z": 1
        }