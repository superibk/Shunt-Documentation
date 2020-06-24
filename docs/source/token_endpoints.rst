Token Endpoints
===============

Token Create
------------

Use this endpoint to obtain user authentication token 

**Default URL**: ``/token/login/``

+----------+----------------------------------+----------------------------------+
| Method   | Request                          | Response                         |
+==========+==================================+==================================+
| ``POST`` | * ``{{ User.USERNAME_FIELD }}``  | ``HTTP_200_OK``                  |
|          | * ``password``                   |                                  |
|          |                                  | * ``auth_token``                 |
+----------+----------------------------------+----------------------------------+

Token Destroy
-------------

Use this endpoint to logout user (remove user authentication token).

**Default URL**: ``/token/logout/``

+----------+----------------+----------------------------------+
| Method   |  Request       | Response                         |
+==========+================+==================================+
| ``POST`` | --             | ``HTTP_204_NO_CONTENT``          |
+----------+----------------+----------------------------------+
