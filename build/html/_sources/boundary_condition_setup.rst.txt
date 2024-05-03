Boundary Condition Setup
======================

.. code-block:: python

    class BoundaryConditionSetup(NamedTuple):

        primitives: BoundaryConditionsField = None     
        levelset: BoundaryConditionsField = None     
        particles: BoundaryConditionsField = None    


* ``primitives``: Define the conditions on primitive variables (u,v,w,p,rho)
* ``levelset``, ``particles``:  Repeat the same for levelset and particles in accordance to the problem. 
Since in the current example "moving fluid and fixed solid with no deformation" exists, Levelset is used.

Boundary Conditions Field
-------------------------

.. code-block:: python

    class BoundaryConditionsField(NamedTuple):

        east: Tuple[BoundaryConditionsFace] = None    
        west: Tuple[BoundaryConditionsFace] = None
        north: Tuple[BoundaryConditionsFace] = None
        south: Tuple[BoundaryConditionsFace] = None
        top: Tuple[BoundaryConditionsFace] = None
        bottom: Tuple[BoundaryConditionsFace] = None


Boundary Conditions Face
^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    class BoundaryConditionsFace(NamedTuple):
    
        boundary_type: str = None  
        bounding_domain_callable: Callable = None     
        primitives_callable: NamedTuple = None      
        wall_velocity_callable: VelocityCallable = None     
        wall_temperature_callable: Callable = None          
        wall_mass_transfer: WallMassTransferSetup = None    
        primitives_table: PrimitivesTable = None        

* ``bounfary_type``: Choose according to the following list:
    * ``PRIMITIVES_TYPES``: ("ZEROGRADIENT", "SYMMETRY", "PERIODIC", "INACTIVE", "WALL", "ISOTHERMALWALL", "MASSTRANSFERWALL", "ISOTHERMALMASSTRANSFERWALL", "DIRICHLET", "NEUMANN")
    * ``LEVELSET_TYPES``: ("ZEROGRADIENT", "SYMMETRY", "PERIODIC", "INACTIVE","DIRICHLET", "REINITIALIZATION")
    * ``PARTICLE_TYPES``: ("ZEROGRADIENT", "SYMMETRY", "PERIODIC", "INACTIVE")

* ``bounding_domain_callable``: (?)
* ``primitives_callable``: Define primitives at the boundry.
* ``wall_velocity_callable``, ``wall_temperature_callable``, and ``wall_mass_transfe``: Define if the boundary is a wall.
* ``primitives_table``: Concatenation of primitives and axes names and values. (?)

Below, the example setup for the domain is shown.

.. code-block:: python

    "boundary_conditions":{
        "primitives":{
            "east":{
                "type":"ZEROGRADIENT"
            },
            "west":{
                "type": "DIRICHLET",
                "primitives_callable":{
                    "rho": 1.0,
                    "u": 100.0,
                    "v": 0.0,
                    "w": 0.0,
                    "p": 1.0
                }

            },
            "north":{
                "type": "ZEROGRADIENT"
            },
            "south":{
                "type": "ZEROGRADIENT"
            },
            "top":{
                "type": "INACTIVE"
            },
            "bottom":{
                "type": "INACTIVE"
            }
        },
        "levelset": {
            "east": {"type": "ZEROGRADIENT"},
            "west": {"type": "ZEROGRADIENT"},
            "north": {"type": "ZEROGRADIENT"},
            "south": {"type": "ZEROGRADIENT"},
            "top": {"type": "INACTIVE"},
            "bottom": {"type": "INACTIVE"}
        }
    },