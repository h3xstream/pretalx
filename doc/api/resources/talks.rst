.. spelling:: de

Talks
=====

Resource description
--------------------

The talk resource is the same as the submission resource, but will limit the returned
submissions to talks that already have a slot on the current schedule. It contains the
following public fields:

.. rst-class:: rest-resource-table

===================================== ========================== =======================================================
Field                                 Type                       Description
===================================== ========================== =======================================================
code                                  string                     A unique, alphanumeric identifier, also used in URLs
speakers                              list                       A list of speaker objects, e.g. ``[{"name": "Jane", "code": "ABCDEF", "biography": ""}]``
title                                 string                     The submission's title
submission_type                       string                     The submission type (e.g. "talk", "workshop")
track                                 string                     The track this talk belongs to (e.g. "security", "design", or ``null``)
state                                 string                     The submission's state, one of "submitted", "accepted", "rejected", "confirmed"
abstract                              string                     The abstract, a short note of the submission's content
description                           string                     The description, a more expansive description of the submission's content
duration                              number                     The talk's duration in minutes, or ``null``
do_not_record                         boolean                    Indicates if the speaker consent to recordings of their talk
is_featured                           boolean                    Indicates if the talk is show in the schedule preview / sneak peek
content_locale                        string                     The language the submission is in, e.g. "en" or "de"
slot                                  object                     An object with the scheduling details, e.g. ``{"start": …, "end": …, "room": "R101"}`` if they exist.
answers                               list                       The question answers given by the speakers. Available if the requesting user has organiser permissions.
===================================== ========================== =======================================================

Endpoints
---------

.. http:get:: /api/events/{event}/talks

   Returns a list of all scheduled submissions the authenticated user/token has access to, or
   all confirmed, publicly scheduled submissions for unauthenticated users.
   For a list of all submissions regardless of their state, authenticated users may choose
   to use the ``/api/events/{event}/submissions`` endpoint instead.

   **Example request**:

   .. sourcecode:: http

      GET /api/events/sampleconf/talks HTTP/1.1
      Accept: application/json, text/javascript

   **Example response**:

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Vary: Accept
      Content-Type: application/json

      {
        "count": 1,
        "next": null,
        "previous": null,
        "results": [
          {
            "code": "ABCDE",
            "speakers": [{"name": "Jane", "code": "DEFAB", "biography": ""}],
            "title": "A talk",
            "submission_type": "talk",
            "state": "confirmed",
            "abstract": "A good talk.",
            "description": "I will expand upon the properties of the talk, primarily its high quality.",
            "duration": 30,
            "do_not_record": true,
            "is_featured": false,
            "content_locale": "en",
            "slot": {
              "start": "2017-12-27T10:00:00Z",
              "end": "2017-12-27T10:30:00Z",
              "room": "R101"
            }
          },
          "answers": [
            {
              "id": 1,
              "question": {"id": 1, "question": {"en": "How much do you like green, on a scale from 1-10?"}, "required": false, "target": "submission", "options": []},
              "answer": "11",
              "answer_file": null,
              "submission": "ABCDE",
              "person": null,
              "options": []
            }
           ]
        ]
      }

   :param event: The ``slug`` field of the event to fetch
   :query page: The page number in case of a multi-page result set, default is 1
   :query q: Search through submissions by title and speaker name
   :query submission_type: Filter submissions by submission type
   :query state: Filter submission by state

.. http:get:: /api/events/(event)/talks/{code}

   Returns information on one event, identified by its slug.

   **Example request**:

   .. sourcecode:: http

      GET /api/events/sampleconf/talks/ABCDE HTTP/1.1
      Accept: application/json, text/javascript

   **Example response**:

   .. sourcecode:: http

      HTTP/1.1 200 OK
      Vary: Accept
      Content-Type: application/json

      {
        "code": "ABCDE",
        "speakers": [{"name": "Jane", "code": "DEFAB", "biography": ""}],
        "title": "A talk",
        "submission_type": "talk",
        "state": "confirmed",
        "abstract": "A good talk.",
        "description": "I will expand upon the properties of the talk, primarily its high quality.",
        "duration": 30,
        "do_not_record": true,
        "is_featured": false,
        "content_locale": "en",
        "slot": {
          "start": "2017-12-27T10:00:00Z",
          "end": "2017-12-27T10:30:00Z",
          "room": "R101"
        },
        "answers": [
          {
            "id": 1,
            "question": {"id": 1, "question": {"en": "How much do you like green, on a scale from 1-10?"}, "required": false, "target": "submission", "options": []},
            "answer": "11",
            "answer_file": null,
            "submission": "ABCDE",
            "person": null,
            "options": []
          }
         ]
      }

   :param event: The ``slug`` field of the event to fetch
   :param code: The ``code`` field of the submission to fetch
   :statuscode 200: no error
   :statuscode 401: Authentication failure
   :statuscode 403: The requested event does not exist **or** you have no permission to view it.
