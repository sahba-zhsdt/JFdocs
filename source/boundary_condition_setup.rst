Boundary Condition Setup
======================

.. code-block:: python

    class BoundaryConditionSetup(NamedTuple):
        primitives: BoundaryConditionsField = None      # define BC for primitve variables: u,v,w,p,rho based on BC field     # >>>
        levelset: BoundaryConditionsField = None      # ok here is where the problem plays an vital role. we have "moving fluid and fixed solid with no deformation" in our problem.
                                                  # it is a special and simplified case of famous "Fluid-Fluid Levelset"
                                                  # so we define this part, too. But what is the meaning?
        particles: BoundaryConditionsField = None     # we skip this. the problem is not about particles.

Boundary Conditions Field
-------------------------

pay attention to the writing format in the exampele.

.. code-block:: python

    class BoundaryConditionsField(NamedTuple):
        east: Tuple[BoundaryConditionsFace] = None      # >>>
        west: Tuple[BoundaryConditionsFace] = None
        north: Tuple[BoundaryConditionsFace] = None
        south: Tuple[BoundaryConditionsFace] = None
        top: Tuple[BoundaryConditionsFace] = None
        bottom: Tuple[BoundaryConditionsFace] = None


Boundary Conditions Face
^^^^^^^^^^^^^^^^^^^^^^^^
.. code-block:: python

    class BoundaryConditionsFace(NamedTuple):
        boundary_type: str = None     # PRIMITIVES_TYPES = ("ZEROGRADIENT", "SYMMETRY", "PERIODIC", "INACTIVE", "WALL", "ISOTHERMALWALL", "MASSTRANSFERWALL", "ISOTHERMALMASSTRANSFERWALL", "DIRICHLET", "NEUMANN")
                                  # LEVELSET_TYPES = ("ZEROGRADIENT", "SYMMETRY", "PERIODIC", "INACTIVE","DIRICHLET", "REINITIALIZATION")
                                  # PARTICLE_TYPES = ("ZEROGRADIENT", "SYMMETRY", "PERIODIC", "INACTIVE")

        bounding_domain_callable: Callable = None     # what is this? I guess the BC can change and be applicable to a specific part.
        primitives_callable: NamedTuple = None      # if not INACTIVE, ZEROGRADIENT; define primitives at the boundry: u,v,w,p,rho > ofcourse in DIRICHLET they will be constant quantities.
                                                # here, we have DIRICHLET BC in the boundry from which fluid flows. Q: real or non-dimensionalized?
        levelset_callable: Callable = None      # I have no comprehension of this part! what does it mean levelset BC? but we will use it as example.
        wall_velocity_callable: VelocityCallable = None     # these three are needed if "WALL", "ISOTHERMALWALL", "MASSTRANSFERWALL", "ISOTHERMALMASSTRANSFERWALL" are selected.
        wall_temperature_callable: Callable = None          # .
        wall_mass_transfer: WallMassTransferSetup = None    # .
        primitives_table: PrimitivesTable = None      # I think it is just a concatenation of primitives and axes names and values?