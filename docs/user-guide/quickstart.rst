Quickstart
==========

Client configuration
--------------------

Start by importing the Python Client and authentication provider::

  from tamr_unify_client import Client
  from tamr_unify_client.auth import UsernamePasswordAuth

Next, create an authentication provider and use that to create an authenticated client::

  import os

  username = os.environ['UNIFY_USERNAME']
  password = os.environ['UNIFY_PASSWORD']

  auth = UsernamePasswordAuth(username, password)
  unify = Client(auth)

.. warning::
  For security, it's best to read your credentials in from environment variables
  instead of hardcoding them directly into your code.

By default, the client tries to find the Unify instance on ``localhost``.
To point to a different host, set the host argument when instantiating the Client.

For example, to connect to ``10.20.0.1``::

  unify = Client(auth, host='10.20.0.1')

Top-level collections
---------------------

The Python Client exposes 2 top-level collections: Projects and Datasets.

You can access these collections through the client and loop over their members
with simple ``for``-loops.

E.g.::

  for project in unify.projects:
    print(project.name)

  for dataset in unify.datasets:
    print(dataset.name)

Fetch a specific resource
-------------------------

If you know the identifier for a specific resource, you can ask for it directly
via the ``by_resource_id`` methods exposed by collections.

E.g. To fetch the project with ID ``'1'``::

  project = unify.projects.by_resoure_id('1')

Resource relationships
----------------------

Related resources (like a project and its unified dataset) can be accessed
through specific methods.

E.g. To access the Unified Dataset for a particular project::

  ud = project.unified_dataset()

Kick-off Unify Operations
-------------------------

Some methods on Model objects can kick-off long-running Unify operations.

Here, kick-off a "Unified Dataset refresh" operation::

  operation = project.unified_dataset().refresh()
  assert op.succeeded()

By default, the API Clients expose a synchronous interface for Unify operations.
