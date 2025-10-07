In this example, we introduce new users to the basic usage of the ``metatrain`` platform through a simple example of training a model from scratch.
The architecture used in this demonstration is **PET**, which will be trained on energies, forces, and stresses stored in a configuration file named ``dataset.extxyz``.
Before starting the training, it is necessary to prepare an ``options.yaml`` file that contains all the information required to set up the training procedure.  
In this file, users can specify training parameters such as the number of epochs, adjust hyperparameters (if needed), and define which targets the model should learn.
Below is an example of such a configuration file:

.. code-block:: yaml

   seed: 42
   device: cuda

   architecture:
     name: pet
     training:
       checkpoint_interval: 10
       distributed: true
       num_epochs: 1000

   training_set:
     systems:
       read_from: dataset_silica_water_NNIP.xyz
       reader: ase
       length_unit: angstrom
     targets:
       energy:
         key: "energy"
         read_from: dataset_silica_water_NNIP.xyz
         reader: ase
         unit: "eV"
         forces:
           key: "forces"
           read_from: dataset_silica_water_NNIP.xyz
           reader: ase

   test_set: 0.1
   validation_set: 0.1

In this example, the ``ase`` library is used to parse all the necessary information about the atomic configurations, energies, and forces from the input dataset.
Finally, it is worth noting that in the ``options.yaml`` file the ``checkpoint_interval`` parameter is set to ``10``.  
This means that the training procedure will save a checkpoint of the model every 10 epochs.
As a final important detail, all the training outputs are automatically organized in an ``outputs`` directory.  
Inside this directory, subfolders are created following the pattern ``/YYYY-MM-DD/HOURS-MINUTES-SECONDS/``, which depends on the exact time when the training is started.
Once these aspects are clarified, the training can be launched from scratch by executing the following command:

.. code-block:: console

   mtt train options.yaml