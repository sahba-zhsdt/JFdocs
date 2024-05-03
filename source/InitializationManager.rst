Initialization Manager
==============

The InitializationManager class implements functionality to create a dictionary of initial buffers that is passed to the simulate() method of the SimulationManager class. 
The initialization() method returns this dictionary. The initial buffers are created in one of the following ways: 
1. From a restart file that is specified in the case setup.
2. From turbulent initial condition parameters that are specified in the case setup
3. From the initial primitive buffer that is passed to the initialization() method
4. From the initial conditions for primitive variables specified in case setup

.. note::
    Note that if multiple of the above are provided, the priority is 1) - 4).