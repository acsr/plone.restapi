Translations
============

Since Plone 5 the product `plone.app.multilingual`_ is included in the base
Plone installation although it is not enabled by default. plone.restapi
provides a `@translations` endpoint to handle the translation information
of the content objects.

Once we have installed `plone.app.multilingual`_ and enabled more than one
language we can link two content-items of different languages to be the
translation of each other issuing a `POST` query to the `@translations`
endpoint including the `id` of the content which should be linked to. The
`id` of the content must be a full URL of the content object:

.. example-code::

  .. code-block:: http-request

    POST /en/test-document/@translations HTTP/1.1
    Host: localhost:8080
    Accept: application/json
    Content-Type: application/json

    {
        'id': 'http://localhost:8080/plone/es/test-document',
    }

  .. code-block:: curl

    curl -i -H "Accept: application/json" -H "Content-Type: application/json" --data-raw '{"id": "http://localhost:8080/Plone/es/test-document"}' --user admin:admin -X POST http://localhost:8080/Plone/en/test-document/@translations

  .. code-block:: httpie

    http -a admin:admin POST http://localhost:8080/Plone/en/test-document/@translations \\id=http://localhost:8080/Plone/es/test-document Accept:application/json Content-Type:application/json

  .. code-block:: python-requests

    requests.post('http://localhost:8080/Plone/en/test-document/@translations', auth=('admin', 'admin'), headers={'Accept': 'application/json', 'Content-Type': 'application/json'}, params={'id': 'http://localhost:8080/Plone/es/test-document'})

.. note::
    "id" is a required field and needs to point to an existing content on the site.

The API will return a `201 Created` response if the linking was successful.


.. literalinclude:: _json/translations_post.json
   :language: js


After linking the contents we can get the list of the translations of that
content item by issuing a ``GET`` request on the `@translations` endpoint of
that content item.:

.. literalinclude:: _json/translations_get.json
   :language: js


To unlink the content, issue a ``DELETE`` request on the `@translations`
endpoint of the content item and provide the language code you want to unlink.:

.. example-code::

  .. code-block:: http-request

    DELETE /en/test-document/@translations HTTP/1.1
    Host: localhost:8080
    Accept: application/json
    Content-Type: application/json

    {
        'language': 'es',
    }

  .. code-block:: curl

    curl -i -H "Accept: application/json" -H "Content-Type: application/json" --data-raw '{"language": "es"}' --user admin:admin -X DELETE http://localhost:8080/Plone/en/test-document/@translations

  .. code-block:: httpie

    http -a admin:admin DELETE http://localhost:8080/Plone/en/test-document/@translations \\language=es Accept:application/json Content-Type:application/json

  .. code-block:: python-requests

    requests.delete('http://localhost:8080/Plone/en/test-document/@translations', auth=('admin', 'admin'), headers={'Accept': 'application/json', 'Content-Type': 'application/json'}, params={'language': 'es'})

.. note::
    "language" is a required field.


.. literalinclude:: _json/translations_delete.json
   :language: js


.. _`plone.app.multilingual`: https://pypi.python.org/pypi/plone.app.multilingual
