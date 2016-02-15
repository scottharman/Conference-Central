Conference Central application - part of the Udacity Fullstack Nano degree

The intent of this is to provide a location where users can register conferences with details about name, location - then create sessions where users are able to add them to a list of sessions they'd be interested in attending. Currently this has only the API endpoints available, but can be tested via the endpoints tool.  The current build is available [here][8]

# Products
- [App Engine][1]

# Language
- [Python][2]

# APIs
- [Google Cloud Endpoints][3]

# Setup Instructions
Application is currently deployed at [udacity-conference-app-1196.appspot.com][4] and can be tested using the [API Explorer][8] You will require a google login to be able to participate.  You will be required to login.

If you wish to run locally or via your own endpoint, you will need to follow the instructions below:
1. Update the value of `application` in `app.yaml` to the app ID you have registered in the App Engine admin console and would like to use to host your instance of this sample.
2. Update the values at the top of `settings.py` to reflect the respective client IDs you have registered in the [Developer Console][5].
3. Update the value of CLIENT_ID in `static/js/app.js` to the Web client ID
4. (Optional) Mark the configuration files as unchanged as follows: `$ git update-index --assume-unchanged app.yaml settings.py static/js/app.js`
5. Run the app with the devserver using `dev_appserver.py DIR`, and ensure it's running by visiting your local server's address (by default [localhost:8080][6].)
6. (Optional) Generate your client library(ies) with [the endpoints tool][7].
7. Deploy your application.

# Task Implementation
## Task 1 - Session class and SessionForm class
Define Session class and SessionForm The Session model contains the following, everything was fairly straightforward, as it wouldn't make sense to have multiple durations, or types of session.   We can have multiple different highlights, and a speaker wouldn't necessarily have to be an entity.  I decided not to implement a speaker class

   `Session name`  _string_ **_complete_**

   `highlights` _array of strings_ **_complete_**

   `speaker` _string_ **_clarified name of speaker_**

   `duration` _int field_ **_complete_**

   `typeOfSession` _string_ _**This was annoying - enums weren't behaving for me.**_

   `start date` _date_ **_complete_** - defaults to today, unless set

   `start time (in 24 hour notation so it can be ordered).` _time field_ implicitly orderable

## Added Endpoints
Implentation of the Session endpoints - these are the required endpoints for basic session manipulation.  getSessionsBySpeaker takes a string, and I am not currently planning to implement a speaker class.  In all cases except for `getSessionsBySpeaker` the endpoint takes a `websafeConferenceKey` which is accessible through the Conference object, so provided you can find a conference, it's trivial to locate the Key required to use as a parent when creating the session.

  `getConferenceSessions(websafeConferenceKey)` -- Given a conference, return all sessions **_complete_**

  `getConferenceSessionsByType(websafeConferenceKey`, typeOfSession) Given a conference, return all sessions of a specified type (eg lecture, keynote, workshop) **_complete_**

  `getSessionsBySpeaker(speaker)`-- Given a speaker, return all sessions given by this particular speaker, across all conferences **_complete_**

  `createSession(SessionForm, websafeConferenceKey)` -- open to the organizer of the conference **_complete_**

## Feature Implementation
Sessions are effectively a stub of conferences, in the sense that a conference can have none or more sessions associated with it, and a session must always belong to one and only one conference. This makes design relatively straightforward. Speakers do not need to be associated with a Conference, and have a one to many relationship with sessions. I've not tied them to system users, as there is no logical reason why they have to be participants in any other part, but this could be a simple enhancement. This meant I could go straight from implementation of the Sessions, to then create my two tasks, which were to send an email on session creation, and also to set the featured speaker.  I was finding it difficult to set the featured speaker from values, so I duplicated the email task first so I could get feedback on my changes fairly quickly and easily

## Add Sessions to User Wishlist
Define the following Endpoints methods

`addSessionToWishlist(SessionKey)`-- adds the session to the user's list of sessions they are interested in attending, ignoring duplicated values

`getSessionsInWishlist()` -- query for all the sessions in a conference that the user is interested in, which have been added to the user's profile.

`deleteSessionInWishlist(SessionKey)` -- removes the session from the user's list of sessions they are interested in attending.  Handle this gracefully

### Additional Features
I decided it would be useful to be able to emai the session organiser when a session was created, this way they have immediate feedback that this has been done, and have a record of key information. This was initially an exercise so I could understand more about tasks and as preparation for writing the code for getFeaturedSpeaker() and the task queue.
- When adding a new session to a conference, determine whether or not the session's speaker should be the new featured speaker. This should be handled using App Engine's Task Queue. **_complete_**
- Define the following endpoints method: `getFeaturedSpeaker()` **_complete_**

## Come up with 2 additional queries
### Query Problem
Solve the following query related problem: Let's say that you don't like workshops and you don't like sessions after 7 pm. How would you handle a query for all non-workshop sessions before 7 pm? What is the problem for implementing this query? What ways to solve it did you think of?

The issue with the query is that the engine itself can't take more than one property for an inequality filter.  This means that anything other than `==` is an inequality filter.  Initially I had assumed that I could find all workshops after 7pm, and take the inverse, but the engine rejected all attempts even though it should logically succeed. As an alternative, I had to break it out into a single in equality filter then remove the workshops explicitly in a `for` loop.

### Additional Queries:
`getSessionsNoSpeaker` Return all the sessions for which we don't have a known speaker - as a future enhancement provide the ability to edit the sessions after creation. Some of these values might be legitimate placeholders, so you might not have confirmation of speaker, but would be required to book a venue ahead of time.

`getAllConferencesWithoutSessions` Return all the conferences which don't have any sessions against them, to allow an organiser to fill in some of the holes that will exist around given topics.

`getAllShortSessions` Return all sessions under 60 minutes long, to allow the user to select something to fill in any gaps in their schedules.

## Notes on implementation:
Struggled with debugging initially, as in Chrome the behaviour of the local instance changed fairly drastically the moment I was adding new ndb entities. Also = lots of finger trouble led to almost constant misspellings of ndb. I did struggle with debugging in the app engine, as I kept losing trace of the log locations, and having to jump around to get entity contents is much harder than I'm used to.  Coming from a traditional rdbms background, I think debugging this way is much harder, as I would simply set my breakpoints in code, tail any logs that I set up, and turn on the general query log in the database engine to make sure my inserts are rational.  The app engine abstraction makes it much harder to create a single unified debugging environment - so I'd like to see if people have a solution for that.

[1]: https://developers.google.com/appengine
[2]: http://python.org
[3]: https://developers.google.com/appengine/docs/python/endpoints/
[4]: http://udacity-conference-app-1196.appspot.com
[5]: https://console.developers.google.com/
[6]: https://localhost:8080/
[7]: https://developers.google.com/appengine/docs/python/endpoints/endpoints_tool
[8]: https://udacity-conference-app-1196.appspot.com/_ah/api/explorer
