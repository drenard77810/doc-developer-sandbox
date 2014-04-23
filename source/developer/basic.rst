.. _basic:

Hands-On Exercise: a basic workflow
###################################

In this hands-on we will prepare a simple workflow and we will execute a first demo run:

* First of all, go to the application default path:

.. code-block:: bash

  $ cd $_CIOP_APPLICATION_PATH

.. NOTE::
 If not otherwise specified, all the commands of these hands-on refer to the $_CIOP_APPLICATION_PATH path.  


Prepare the workflow
^^^^^^^^^^^^^^^^^^^^

A workflow is a DAG [#f1]_. There is a special file, named *application.xml*, that defines a workflow. The first step is to create an *application.xml*:
 
* Create a file named *application.xml*:

.. code-block:: bash

 $ touch application.xml
 
* Give to your user/group the read/write permissions:

.. code-block:: bash

 $ chmod 664 application.xml
 
* Open it with a text editor (e.g. vi) and paste the following code:

.. literalinclude:: src/basic/application.xml
  :language: xml
  :tab-width: 2

We used the id  *beam_arithm* because it will be useful later on.

Prepare the test inputs
^^^^^^^^^^^^^^^^^^^^^^^
 
* Create a file named list:

.. code-block:: bash

 $ mkdir inputs
 $ touch inputs/list
 
* Open it with a text editor and paste the following lines:

.. code-block:: bash

 file1
 file2

.. NOTE::
 The file should contain only the two lines, without blank lines at the end or at the beginning. Furthermore, comments are not allowed.
 
 
Prepare the streaming executable
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The streaming executable is responsbile for *executing* your application in a node of a cluster. In the *application.xml* we defined a workflow with a single node and the related streaming executable:

.. literalinclude:: src/basic/application.xml
  :language: xml
  :tab-width: 2
  :lines: 5-5

* Go to the application default path and create the executable directory:

.. code-block:: bash

 $ cd /application
 $ mkdir expression
 $ cd expression

We used the name *expression* because it is a step of the BEAM Arithm workflow.
 
* Create a file named *run* and make it executable:

.. code-block:: bash

 $ touch run
 $ chmod +x run
 
* Open it with a text editor and paste the following code:

.. literalinclude:: src/basic/run
  :language: bash
  :tab-width: 2

Run the node 
^^^^^^^^^^^^

We created a workflow with a single node, named *expression*. We can execute it by typing:

.. code-block:: bash

 $ ciop-simjob node_expression

The output will be similar to:

.. code-block:: none
 
 14/04/22 11:07:19 INFO node_expression simulation started
 14/04/22 11:07:31 INFO Submitting job 18251 ...
 14/04/22 11:07:31 WARN streaming.StreamJob: -jobconf option is deprecated, please use -D instead.
 packageJobJar: [/var/lib/hadoop-0.20/cache/crossi/hadoop-unjar6138465056018141051/] [] /tmp/streamjob8303102846061764954.jar tmpDir=null
 14/04/22 11:07:33 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
 14/04/22 11:07:33 WARN snappy.LoadSnappy: Snappy native library not loaded
 14/04/22 11:07:33 INFO mapred.FileInputFormat: Total input paths to process : 1
 14/04/22 11:07:35 INFO streaming.StreamJob: getLocalDirs(): [/var/lib/hadoop-0.20/cache/crossi/mapred/local]
 14/04/22 11:07:35 INFO streaming.StreamJob: Running job: job_201404181621_0001
 14/04/22 11:07:35 INFO streaming.StreamJob: To kill this job, run:
 14/04/22 11:07:35 INFO streaming.StreamJob: /usr/lib/hadoop-0.20/bin/hadoop job  -Dmapred.job.tracker=sb-10-16-10-21.dev.terradue.int:8021 -kill job_201404181621_0001
 14/04/22 11:07:35 INFO streaming.StreamJob: Tracking URL: http://sb-10-16-10-21.dev.terradue.int:50030/jobdetails.jsp?jobid=job_201404181621_0001
 14/04/22 11:07:36 INFO streaming.StreamJob:  map 0%  reduce 0%
 14/04/22 11:07:41 INFO streaming.StreamJob:  map 100%  reduce 0%
 14/04/22 11:07:50 INFO streaming.StreamJob:  map 100%  reduce 100%
 14/04/22 11:07:52 INFO streaming.StreamJob: Job complete: job_201404181621_0001
 14/04/22 11:07:52 INFO streaming.StreamJob: Output: /tmp/sandbox/beam_arithm/node_expression/output
 14/04/22 11:07:52 INFO node_expression simulation ended (33 seconds)
 14/04/22 11:07:52 INFO node_expression published:
 
 14/04/22 11:07:52 INFO The results are available at /share/tmp/sandbox/beam_arithm/node_expression/data

What we done
^^^^^^^^^^^^

#. We created a simple workflow with a single node,
#. We prepared a list of two test inputs,
#. We prepared a simple streaming executable that logs (through the *ciop-log* function) the name of the inputs,
#. We executed the node with two inputs. Since the Sandbox has two cores, they have been executed in parallel.

.. rubric:: Footnotes

.. [#f1] `Directed acyclic graph <http://en.wikipedia.org/wiki/Directed_acyclic_graph>`_