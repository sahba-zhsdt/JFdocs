Initia Condition Setup
======================

.. code-block:: python

    class InitialConditionSetup(NamedTuple):

        primitives: NamedTuple      
        levelset: InitialConditionLevelset    
        solid_velocity: VelocityCallable     
        particles: InitialConditionParticles     
        turbulent: InitialConditionTurbulent      
        cavitation: InitialConditionCavitation     
        is_turbulent: bool      
        is_cavitation: bool     

If the problem is not turbulent, then the `is_turbulent` is False. The same for cavitation.
Accordingly, the `turbulent` and `cavitation` are not defined.
Also,``solid_velocity`` is used for a moving solid. 


Initial Condition Levelset
--------------------------

There are four options to define from which function the levelset is used.
Choose one of them and set the corresponding boolean to True. 
Some pre-defined shapes are available in the ``shape_dictionary`` in the ``InitialLevelsetBlock`` class.

.. code-block:: python

    class InitialConditionLevelset(NamedTuple):
        blocks: Tuple[InitialLevelsetBlock]     
        levelset_callable: Callable
        h5_file_path: str
        NACA_profile: str    
        is_blocks: bool
        is_callable: bool
        is_NACA: bool
        is_h5_file: bool


Initial Levelset Block
^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python
    
    class InitialLevelsetBlock(NamedTuple):
        shape: str      # shape_dictionary = {
        # "circle": CircleParameters,
        # "sphere": SphereParameters,
        # "square": SquareParameters,
        # "rectangle": RectangleParameters,
        # "diamond": DiamondParameters,
        # "ellipse": EllipseParameters,
        # "ellipsoid": EllipsoidParameters,
        # }
        parameters: NamedTuple
        bounding_domain_callable: Callable

Below, the example setup for the domain is shown.

.. code-block:: python

    "initial_condition":{
        "primitives":{
            "rho": 1.0,
            "u": 100.0,
            "v": 0.0,
            "w": 0.0,
            "p": 1.0
        },
        "levelset":"lambda x,y: - 0.5 + jnp.sqrt(x**2 + y**2)"
           
    },