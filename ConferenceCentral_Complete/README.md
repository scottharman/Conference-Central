App Engine application for the Udacity training course.

# Products
- [App Engine][1]

# Language
- [Python][2]

# APIs
- [Google Cloud Endpoints][3]

# Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you
2. have registered in the App Engine admin console and would like to use to host
3. your instance of this sample.
4. Update the values at the top of `settings.py` to
5. reflect the respective client IDs you have registered in the
6. [Developer Console][4].
7. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
8. (Optional) Mark the configuration files as unchanged as follows:
9. `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
10. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
11. (Optional) Generate your client library(ies) with [the endpoints tool][6].
12. Deploy your application.

## Notes on implementation:
Struggled with debugging initially, as in Chrome the behaviour of the local instance changed fairly drastically the moment I was adding new ndb entities. Also = lots of finger trouble led to almost constant misspellings of ndb.

# Tasks from rubric:
1. Add Sessions to a Conference
- Define the following endpoint methods:

   `getConferenceSessions(websafeConferenceKey)` -- Given a conference, return all sessions

   `getConferenceSessionsByType(websafeConferenceKey`, typeOfSession) Given a conference, return all sessions of a specified type (eg lecture, keynote, workshop)

   `getSessionsBySpeaker(speaker)`-- Given a speaker, return all sessions given by this particular speaker, across all conferences

   `createSession(SessionForm, websafeConferenceKey)` -- open to the organizer of the conference

- Define Session class and SessionForm

   `Session name`

    `highlights`

   `speaker`

   `duration`

   `typeOfSession` * This was annoying - enums weren't behaving for me.

   `date`

   `start time (in 24 hour notation so it can be ordered).`

- Add Sessions to User Wishlist

  Define the following Endpoints methods

  `addSessionToWishlist(SessionKey)`-- adds the session to the user's list of sessions they are interested in attending

  `getSessionsInWishlist()` -- query for all the sessions in a conference that the user is interested in

  `deleteSessionInWishlist(SessionKey)` -- removes the session from the user's list of sessions they are interested in attending

- Work on indexes and queries

   Create indexes

   Come up with 2 additional queries

   Solve the following query related problem: Let's say that you don't like workshops and you don't like sessions after 7 pm. How would you handle a query for all non-workshop sessions before 7 pm? What is the problem for implementing this query? What ways to solve it did you think of?

- Add a Task
2. When adding a new session to a conference, determine whether or not the session's speaker should be the new featured speaker. This should be handled using App Engine's Task Queue.
3. Define the following endpoints method: `getFeaturedSpeaker()`

[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
