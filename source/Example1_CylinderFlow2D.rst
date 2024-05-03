Example 1: Cylinder Flow in 2D
==============================

Cylinder Flow, also known as a Kármán vortex street, is one of the fundemental problems in fluid mechanics.
Considering Reynolds number in different ranges, the cylinder wake behaves differently. 
For more details refer to `this paper <https://doi.org/10.1146/annurev.fl.28.010196.002401>`_.
The example files are located in the:

* to the developers
 ``nlfvs/examples/examples_2D/08_cylinderflow`` 

 ``nlfvs/tests/levelset/solids/cylinderflow``
* to the users
 ``JAXFLUIDS/examples/examples_2D/08_cylinderflow``


In each directory, a single python file and two json files are provided. The starting point is the python file.
The first classes to be imported are:

.. code-block:: python

    from jaxfluids import InputManager, InitializationManager, SimulationManager


The InputManager takes the ``.json`` files as input; 
afterwards, the output is fed to the InitializationManager and the SimulationManager.

.. code-block:: python

    # SETUP SIMULATION
    input_manager = InputManager("case_setup.json", "numerical_setup.json")
    initialization_manager = InitializationManager(input_manager)
    sim_manager = SimulationManager(input_manager)

The input ``.json`` files are discussed here:

.. toctree::
    :maxdepth: 2

    CaseSetup
    NumericalSetup

Next, to run the simulation, ``InitializationManager.initialization()``
and ``SimulationManager.simulate()`` are used:

.. code-block:: python

    # RUN SIMULATION
    simulation_buffers, time_control_variables,\
    forcing_parameters = initialization_manager.initialization()
    sim_manager.simulate(simulation_buffers, time_control_variables) 

The ``initialization()`` function returns ???
The ``simulate()`` function returns ???

Once the simulation is complete, the results can be visualized using tools from postprocessing module
as shown the example below:

.. code-block:: python

    from jaxfluids_postprocess import load_data, create_2D_animation, create_2D_figure

This step, Data Loading and Plotting, depends on the problem and its outputs.
Therefore, it is recommended to check ``.json`` files in advance.
Then, refer to the full ``.py`` file including all steps.  

.. toctree::
    :maxdepth: 1

    CylinderFlow 





