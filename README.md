App Engine application for the Udacity training course.

# Products
- [App Engine][1]

# Language
- [Python][2]

# APIs
- [Google Cloud Endpoints][3]

# Setup Instructions
1. Update the value of `application` in `app.yaml` to the app ID you have registered in the App Engine admin console and would like to use to host your instance of this sample.
2. Update the values at the top of `settings.py` to reflect the respective client IDs you have registered in the [Developer Console][4].
3. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
4. (Optional) Mark the configuration files as unchanged as follows: `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
5. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][5].)
6. (Optional) Generate your client library(ies) with [the endpoints tool][6].
7. Deploy your application.

## Notes on implementation:
Struggled with debugging initially, as in Chrome the behaviour of the local instance changed fairly drastically the moment I was adding new ndb entities. Also = lots of finger trouble led to almost constant misspellings of ndb. I did struggle with debugging in the app engine, as I kept losing trace of the log locations, and having to jump around to get entity contents is much harder than I'm used to.  Coming from a traditional rdbms background, I think debugging this way is much harder, as I would simply set my breakpoints in code, tail any logs that I set up, and turn on the general query log in the database engine to make sure my inserts are rational.  The app engine abstraction makes it much harder to create a single unified debugging environment - so I'd like to see if people have a solution for that.

## Feature Implementation
Sessions are effectively a stub of conferences, in the sense that a conference can have none or more sessions associated with it, and a session must always belong to one and only one conference. This makes design relatively straightforward. Speakers do not need to be associated with a Conference, and have a one to many relationship with sessions. I've not tied them to system users, as there is no logical reason why they have to be participants in any other part, but this could be a simple enhancement. This meant I could go straight from implementation of the Sessions, to then create my two tasks, which were to send an email on session creation, and also to set the featured speaker.  I was finding it difficult to set the featured speaker from values, so I duplicated the email task first so I could get feedback on my changes fairly quickly and easily

# Tasks from rubric:
1. Add Sessions to a Conference
- Define the following endpoint methods:

   `getConferenceSessions(websafeConferenceKey)` -- Given a conference, return all sessions **_complete_**

   `getConferenceSessionsByType(websafeConferenceKey`, typeOfSession) Given a conference, return all sessions of a specified type (eg lecture, keynote, workshop) **_complete_**

   `getSessionsBySpeaker(speaker)`-- Given a speaker, return all sessions given by this particular speaker, across all conferences **_complete_**

   `createSession(SessionForm, websafeConferenceKey)` -- open to the organizer of the conference **_complete_**

- Define Session class and SessionForm

   `Session name`  _string_ **_complete_**

    `highlights` _array of strings_ **_complete_**

   `speaker` _string_ **_complete_**

   `duration` _int field_ **_complete_**

   `typeOfSession` _string_ _**This was annoying - enums weren't behaving for me.**_

   `start date` _date_ **_complete_** - defaults to today, unless set

   `start time (in 24 hour notation so it can be ordered).` _time field_ implicitly orderable

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
2. When adding a new session to a conference, determine whether or not the session's speaker should be the new featured speaker. This should be handled using App Engine's Task Queue. **_complete_**
3. Define the following endpoints method: `getFeaturedSpeaker()` **_complete_**

## Added additional task - email when session is Added
Did this to understand how to create tasks with parameters

[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: https://console.developers.google.com/
[5]: https://localhost:8080/
[6]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
