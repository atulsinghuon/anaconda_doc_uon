Anaconda
========

This documentation discusses the various aspects of using Anaconda on Ada. 

Quickstart
----------

Anaconda module can be loaded using the following command. 

.. code-block:: bash

    module load anaconda-uoneasy/2023.09-0


Once the module is loaded conda becomes available and hence usual conda commands can now be used. For instance, creating a new conda environement

Create a conda env
------------------

.. code-block:: bash

    conda create --name <my_awesome_environment>

This environement can now be activated using the source command as follows, 

.. code-block:: bash

    source activate <my_awesome_environment>


Once the conda environment is activated, a ``(my_awesome_environement)`` keyword appears at the shell indicating the environement as active. 

Some conda commands
-------------------
The required packages can now be installed in the environment using the usual conda commands as follows, 

.. code-block:: bash

    #conda package installation commands
    conda install package_name=version_number

    # seek help 
    conda install --help

    #search conda-forge
    conda search package_name --channel conda-forge

    # Create a conda environment with a specific python version (say 3.7)
    conda create -name environment_name python=3.7 package_name

Similarly for bash we have the following usual sets of commands.

Some pip commands
-----------------

.. code-block:: bash

    # pip packages installation commands
    pip install package_name=version_number

    # seek help 
    pip install --help

    # Search Python Index PyPi
    pip search gpy

    # Install using a specific python version = 3.8
    pip install python=3.8 numpy scipy

    # Install using a requirements file
    pip install -r requirements.txt

    # Uninstall a package
    pip uninstall package_name


Various other conda commands can be used as usual, like checking the python version, listing of packages currently installed and checking if a specific package is installed or not.


.. code-block:: bash

    # Check the python version 
    $ python --version

    # Check the list of installed packages
    $ pip list 

    # Check the package version
    $ pip show package_name

See the `pip documentation <https://pip.pypa.io/en/stable/user_guide/>`_ for more commands.

Environment using virtualenv
----------------------------

Virtual env can also be used to create environements, but first it needs to be installed. 

.. code-block:: bash

    #load anaconda
    module load anaconda-uoneasy/2023.09-0

    #install virtualenv
    pip install --user virtualenv

Once the virtual env is installed, a new directory needs to be created for loading all the packages in the directory and then sourcing the same. 

.. code-block:: bash

    #create a new directory of the environement name
    mkdir myenv

    # user virtualenv
    virtualenv myenv

    # source the directory
    source myenv/bin/activate


This should give the ``(myenv)`` at the shell, indicating the virtualenv has been successfully created. Python packages can now be installed as usual using pip. 

.. code-block:: bash

    # Insall a package
    pip install package_name

    # close the virtualenv
    deactivate


Submit a python job using SLURM
-------------------------------

Once "all" the necessary python packages have been installed here in the conda environement, one can now submit jobs to the cluster, using the following SLURM commands. 


.. tabs::

    .. code-tab:: bash SLURM

        #!/bin/bash
        #SBATCH --job-name=pyjob            # create a short name
        #SBATCH --nodes=1                   # node count
        #SBATCH --ntasks=1                  # total number of tasks across all nodes
        #SBATCH --cpus-per-task=1           # >1 if multi-threaded tasks
        #SBATCH --mem-per-cpu=4G            # memory per cpu-core (4G is the default, but can change according to the partitions)
        #SBATCH --time=00:01:00             # time limit of the booked resource (HH:MM:SS)
        #SBATCH --mail-type=begin           # email when job starts
        #SBATCH --mail-type=end             # email when job ends
        #SBATCH --mail-user=<your_email_id> # enter your email 

        module load anaconda-uoneasy/2023.09.0    #load the anaconda module
        source activate my_awesome_environement   # activate the previously created environement

        ### uncomment the below line if environment is created using virtualenv
        #### source </path/to/>my_awesome_environement/bin/activate

        python myscript.py  ## run the python file. 

Note that the module is activated using ``source activate`` instead of the ``conda activate``. And secondly, Running python file in this fashion does not gaurantee utilization of parallelization, unless hard coded within the ``myscript.py``. 

Saving the above file as, say ``submit.slurm``, and then performing the following command will submit the python job on ADA. 

.. code-block:: bash

    sbatch submit.slurm


The recommended way of installing packages for python is through conda. 






