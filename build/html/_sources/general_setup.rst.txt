General Setup
================

.. code-block:: python

    class GeneralSetup(NamedTuple):
    case_name: str     
    end_time: float    
    end_step: int     
    save_path: str      
    save_dt: float     
    save_step: int      
    save_timestamps: np.ndarray     
    save_start: float 


* ``case_name``: Choose a simple name. It will appear later in your directory. 
* ``end_time``:  The time at which the simulation will end. 
* ``end_step``: The step at which the simulation will end.

.. warning::
    You can choose either ``end_time`` or ``end_step``.

* ``save_path``: The path where the data will be saved.
* ``save_dt``: The time interval at which the data will be saved.
* ``save_step``: The step interval at which the data will be saved.
* ``save_timestamps``: A timestamp is a numeric representation of a specific point in time. Use this option a specific time is required.

.. warning::
    You can choose either ``save_dt``, ``save_step``, or ``save_timestamps``.

* ``save_start``: The time at which the data saving will start.(?)

.. note::
    All of the classes are namedtuples. You can access the attributes by using the dot operator. For example, ``GeneralSetup.end_time``. 

Below, the example setup for the domain is shown.

.. code-block:: python

        "general":{
        "case_name": "Karman_Re_100_d",
        "end_time": 900.0,
        "save_dt": 1.0,
        "save_path": "./customized_examples/von_Karman/Re100"
    },