Base Endpoints
==============

User Create
-----------

Use this endpoint to register new user.

**Default URL**: ``/users/``


+----------+-----------------------------------+----------------------------------+
| Method   |  Request                          | Response                         |
+==========+===================================+==================================+
| ``POST`` | * ``{{ User.USERNAME_FIELD }}``   | ``HTTP_201_CREATED``             |
|          | * ``{{ User.REQUIRED_FIELDS }}``  |                                  |
|          | * ``password``                    | * ``{{ User.USERNAME_FIELD }}``  |
|          | * ``re_password``                 | * ``{{ User._meta.pk.name }}``   |
|          |                                   | * ``{{ User.REQUIRED_FIELDS }}`` |
|          |                                   |                                  |
|          |                                   | ``HTTP_400_BAD_REQUEST``         |
|          |                                   |                                  |
|          |                                   | * ``{{ User.USERNAME_FIELD }}``  |
|          |                                   | * ``{{ User.REQUIRED_FIELDS }}`` |
|          |                                   | * ``password``                   |
|          |                                   | * ``re_password``                |
+----------+-----------------------------------+----------------------------------+

User Activate
-------------

Use this endpoint to activate user account. ``HTTP_403_FORBIDDEN`` will be raised if user is already
active when calling this endpoint (this will happen if you call it more than once).

**Default URL**: ``/users/activation/``

+----------+--------------------------------------+----------------------------------+
| Method   | Request                              | Response                         |
+==========+======================================+==================================+
| ``POST`` | * ``uid``                            | ``HTTP_204_NO_CONTENT``          |
|          | * ``token``                          |                                  |
|          |                                      | ``HTTP_400_BAD_REQUEST``         |
|          |                                      |                                  |
|          |                                      | * ``uid``                        |
|          |                                      | * ``token``                      |
|          |                                      |                                  |
|          |                                      | ``HTTP_403_FORBIDDEN``           |
|          |                                      |                                  |
|          |                                      | * ``detail``                     |
+----------+--------------------------------------+----------------------------------+

User Resend Activation E-mail
------------------------------

Use this endpoint to re-send the activation e-mail. Note that no e-mail would
be sent if the user is already active or if they don't have a usable password.

**Default URL**: ``/users/resend_activation/``

+----------+--------------------------------------+----------------------------------+
| Method   | Request                              | Response                         |
+==========+======================================+==================================+
| ``POST`` | * ``{{ User.EMAIL_FIELD }}``         | ``HTTP_204_NO_CONTENT``          |
|          |                                      | ``HTTP_400_BAD_REQUEST``         |
+----------+--------------------------------------+----------------------------------+

User
----

Use this endpoint to retrieve/update the authenticated user.

**Default URL**: ``/users/me/``

+----------+--------------------------------+----------------------------------+
| Method   |           Request              |           Response               |
+==========+================================+==================================+
| ``GET``  |    --                          | ``HTTP_200_OK``                  |
|          |                                |                                  |
|          |                                | * ``{{ User.USERNAME_FIELD }}``  |
|          |                                | * ``{{ User._meta.pk.name }}``   |
|          |                                | * ``{{ User.REQUIRED_FIELDS }}`` |
+----------+--------------------------------+----------------------------------+
| ``PUT``  | ``{{ User.REQUIRED_FIELDS }}`` | ``HTTP_200_OK``                  |
|          |                                |                                  |
|          |                                | * ``{{ User.USERNAME_FIELD }}``  |
|          |                                | * ``{{ User._meta.pk.name }}``   |
|          |                                | * ``{{ User.REQUIRED_FIELDS }}`` |
|          |                                |                                  |
|          |                                | ``HTTP_400_BAD_REQUEST``         |
|          |                                |                                  |
|          |                                | * ``{{ User.REQUIRED_FIELDS }}`` |
+----------+--------------------------------+----------------------------------+
| ``PATCH``| ``{{ User.FIELDS_TO_UPDATE }}``| ``HTTP_200_OK``                  |
|          |                                |                                  |
|          |                                | * ``{{ User.USERNAME_FIELD }}``  |
|          |                                | * ``{{ User._meta.pk.name }}``   |
|          |                                | * ``{{ User.REQUIRED_FIELDS }}`` |
|          |                                |                                  |
|          |                                | ``HTTP_400_BAD_REQUEST``         |
|          |                                |                                  |
|          |                                | * ``{{ User.REQUIRED_FIELDS }}`` |
+----------+--------------------------------+----------------------------------+

User Delete
-----------

Use this endpoint to delete authenticated user. By default it will simply verify
password provided in ``current_password``

**Default URL**: ``/users/me/``

+------------+---------------------------------+----------------------------------+
| Method     |  Request                        | Response                         |
+============+=================================+==================================+
| ``DELETE`` | * ``current_password``          | ``HTTP_204_NO_CONTENT``          |
|            |                                 |                                  |
|            |                                 | ``HTTP_400_BAD_REQUEST``         |
|            |                                 |                                  |
|            |                                 | * ``current_password``           |
+------------+---------------------------------+----------------------------------+

Set Username
------------

Use this endpoint to change user's ``USERNAME_FIELD``.
By default this changes the ``username``.

.. note::

    When you see ``{USERNAME_FIELD}`` or ``{EMAIL_FIELD}`` in the doc below,
    it means that those parts will be substituted with what you set.

**Default URL**: ``/users/set_{USERNAME_FIELD}/``

+----------+----------------------------------------+-------------------------------------------+
| Method   | Request                                | Response                                  |
+==========+========================================+===========================================+
| ``POST`` | * ``new_{USERNAME_FIELD}``             | ``HTTP_204_NO_CONTENT``                   |
|          | * ``re_new_{USERNAME_FIELD}``          |                                           |
|          | * ``current_password``                 | ``HTTP_400_BAD_REQUEST``                  |
|          |                                        |                                           |
|          |                                        | * ``new_{USERNAME_FIELD}``                |
|          |                                        | * ``re_new_{USERNAME_FIELD}``             |
|          |                                        | * ``current_password``                    |
+----------+----------------------------------------+-------------------------------------------+

Reset Username
--------------

Use this endpoint to send email to user with username reset link. 

**Default URL**: ``/users/reset_{USERNAME_FIELD}/``

.. note::

    ``HTTP_204_NO_CONTENT`` is the default Otherwise if the value of ``{EMAIL_FIELD}`` 
    does not exist in database ``HTTP_400_BAD_REQUEST``

+----------+---------------------------------+------------------------------+
| Method   | Request                         | Response                     |
+==========+=================================+==============================+
| ``POST`` |  ``{EMAIL_FIELD}``              | ``HTTP_204_NO_CONTENT``      |
|          |                                 |                              |
|          |                                 | ``HTTP_400_BAD_REQUEST``     |
|          |                                 |                              |
|          |                                 | * ``{EMAIL_FIELD}``          |
+----------+---------------------------------+------------------------------+

Reset Username Confirmation
---------------------------

Use this endpoint to finish reset username process. 
``HTTP_400_BAD_REQUEST`` will be raised if the user has logged in or changed username
since the token creation.

**Default URL**: ``/users/reset_{USERNAME_FIELD}_confirm/``


+----------+----------------------------------+--------------------------------------+
| Method   | Request                          | Response                             |
+==========+==================================+======================================+
| ``POST`` | * ``uid``                        | ``HTTP_204_NO_CONTENT``              |
|          | * ``token``                      |                                      |
|          | * ``new_{USERNAME_FIELD}``       | ``HTTP_400_BAD_REQUEST``             |
|          | * ``re_new_{USERNAME_FIELD}``    |                                      |
|          |                                  | * ``uid``                            |
|          |                                  | * ``token``                          |
|          |                                  | * ``new_{USERNAME_FIELD}``           |
|          |                                  | * ``re_new_{USERNAME_FIELD}``        |
+----------+----------------------------------+--------------------------------------+

Set Password
------------

Use this endpoint to change user password.

**Default URL**: ``/users/set_password/``


+----------+------------------------+-------------------------------------------+
| Method   | Request                | Response                                  |
+==========+========================+===========================================+
| ``POST`` | * ``new_password``     | ``HTTP_204_NO_CONTENT``                   |
|          | * ``re_new_password``  |                                           |
|          | * ``current_password`` | ``HTTP_400_BAD_REQUEST``                  |
|          |                        |                                           |
|          |                        | * ``new_password``                        |
|          |                        | * ``re_new_password``                     |
|          |                        | * ``current_password``                    |
+----------+------------------------+-------------------------------------------+

Reset Password
--------------

Use this endpoint to send email to user with password reset link. 

**Default URL**: ``/users/reset_password/``

.. note::

    ``HTTP_204_NO_CONTENT`` is the default Otherwise if the value of ``{EMAIL_FIELD}`` 
    does not exist in database ``HTTP_400_BAD_REQUEST``

+----------+---------------------------------+------------------------------+
| Method   | Request                         | Response                     |
+==========+=================================+==============================+
| ``POST`` |  ``{EMAIL_FIELD}``              | ``HTTP_204_NO_CONTENT``      |
|          |                                 |                              |
|          |                                 | ``HTTP_400_BAD_REQUEST``     |
|          |                                 |                              |
|          |                                 | * ``{EMAIL_FIELD}``          |
+----------+---------------------------------+------------------------------+

Reset Password Confirmation
---------------------------

Use this endpoint to finish reset password process.
``HTTP_400_BAD_REQUEST`` will be raised if the user has logged in or changed password
since the token creation.

**Default URL**: ``/users/reset_password_confirm/``


+----------+----------------------------------+--------------------------------------+
| Method   | Request                          | Response                             |
+==========+==================================+======================================+
| ``POST`` | * ``uid``                        | ``HTTP_204_NO_CONTENT``              |
|          | * ``token``                      |                                      |
|          | * ``new_password``               | ``HTTP_400_BAD_REQUEST``             |
|          | * ``re_new_password``            |                                      |
|          |                                  | * ``uid``                            |
|          |                                  | * ``token``                          |
|          |                                  | * ``new_password``                   |
|          |                                  | * ``re_new_password``                |
+----------+----------------------------------+--------------------------------------+
