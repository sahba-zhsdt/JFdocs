Initia Condition Setup
======================

.. code-block:: python

    class InitialConditionSetup(NamedTuple):
        primitives: NamedTuple      # u,v,w,p,rho
        levelset: InitialConditionLevelset      # >>> a
        solid_velocity: VelocityCallable      # I guess if solid
        particles: InitialConditionParticles      # we skip. no particles
        turbulent: InitialConditionTurbulent      # we skip. no turbulence
        cavitation: InitialConditionCavitation      # we skip. no cavitation
        is_turbulent: bool      # we skip. no turbulence
        is_cavitation: bool     # we skip. no cavitation

Initial Condition Levelset
--------------------------

basically, I have problem in understanding the meaning of IC in levelset.
bUT, I think we have four options to define from which function the levelset will evolve!
block - NACA - h5 fiel - levelset user defined function. choose one and continue.
in the existing example, this inline lambda function is defined: "levelset": "lambda x,y: - 0.5 + jnp.sqrt(x**2 + y**2)"
I can understand it is a circle (the cross section of the cylinder). so "-0,5" is the distance from free surface?
what is zero-level of this? how the levels will vary by movment of fluid over this cylinder, then?
Is not the cross section between moving fluid and the cylinder a curved surface??
I need to run the file to see the shape!

.. code-block:: python

    class InitialConditionLevelset(NamedTuple):
        blocks: Tuple[InitialLevelsetBlock]     # >>>
        levelset_callable: Callable
        h5_file_path: str
        NACA_profile: str     # it is airfoil. used for ramp flow I think
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