Domain Setup
================

.. code-block:: python

    class DomainSetup(NamedTuple):
        x: AxisSetup   
        y: AxisSetup
        z: AxisSetup
        decomposition: DomainDecompositionSetup     # >>> b
        particle_setup: ParticleSetup     # >>> c
        active_axes: Tuple[str]     # the rest seems to play with axes. we can skip them for now.
        active_axes_indices: Tuple[int]
        inactive_axes: Tuple[str]
        inactive_axes_indices: Tuple[int]
        dim: int      # no idea ?!


Axis Setup
----------------

.. code-block:: python

    class AxisSetup(NamedTuple):
        cells: int      # you choose the number of cells being considered in solver. it depends on the problem. here, according to the existing example we choose "1200" for x and y. (and for z if it  is 3D)
        range: Tuple      # you choose the spatial range. here we choose [-50.0, 200.0]
        stretching: MeshStretchingSetup     # >>>


Mesh Stretching Setup
^^^^^^^^^^^^^^^^^^^^^
In cases that you need more precision, you may add some stretching mesh to the sensitive region.

.. code-block:: python

    class MeshStretchingSetup(NamedTuple):
        type: str     # choose between TUPLE_MESH_STRETCHING_TYPES = ("CHANNEL", "BOUNDARY_LAYER", "PIECEWISE", "BUBBLE_1", "BUBBLE_2") + homogeneous ?? I think it is the default.
        tanh_value: float     # you need it if you choose "CHANNEL" , "BOUNDARY_LAYER"
        ratio_fine_region: float      # you need it if you choose "BUBBLE_1", "BUBBLE_2"
        cells_fine: float     # # you need it if you choose "BUBBLE_1", "BUBBLE_2"
        piecewise_parameters: Tuple[PiecewiseStretchingParameters]      # >>>

Piecewise Stretching Parameters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
if you choose "PIECEWISE", come here. in our problem we need this type. none from above is suitable.
in each axis, divide the range to pieces and define how many cells it will stretch and how.
Strestching means the width of the cell will change. naturally it is dec. const. inc. >>> |   |  | | | | |  |   |    | :)

.. code-block:: python

    class PiecewiseStretchingParameters(NamedTuple):
        type: str choose between TUPLE_PIECEWISE_STRETCHING_TYPES = ("DECREASING", "INCREASING", "CONSTANT")
        cells: int
        upper_bound: float
        lower_bound: float


Domain Decomposition Setup
-------------------------
split the domain (and data) into subdomains that can be solved independently – but require data exchange on the domain boundaries.
I guess it just solves the axes independant from eachother, which it seems we want it! so we add it with the default int=1.

.. code-block:: python

    class DomainDecompositionSetup(NamedTuple):
        split_x: int = 1
        split_y: int = 1
        split_z: int = 1


Particle Setup
--------------
we don't work with particles. skip this part.

.. code-block:: python

    class ParticleSetup(NamedTuple):
        particle_count: int = 0    