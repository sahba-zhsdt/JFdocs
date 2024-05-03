Restart Setup
====================

Use this setup block to restart an existing simulation.

.. code-block:: python

    class RestartSetup(NamedTuple):
    flag: bool
    file_path: str     
    use_time: bool
    time: float
    is_equal_decomposition: bool


* ``flag``: (?) 
* ``file_path``: Path to an existing ``.h5`` file. This file will be used to continue the simulation.
* ``use_time``: If you want to continue the simulation from a specific time, set this to ``True``. 
* ``time``: The time from which you want to continue the simulation. 
* ``is_equal_decomposition``: If the decomposition of the existing file is the same as the current decomposition, set this to ``True``. (?)

.. note::
    All of the classes are namedtuples. You can access the attributes by using the dot operator. 