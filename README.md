# Event-Scheduler-

# Description

This is an event booking application that allows users to register or rather RSVP for a particular event. Registered users would be able receive updates about the event as well as cancel their attendance. Primarily, JavaScript was used to create this project for its simplicity and ease in unifying with web components. Some HTML and CSS was implemented to bring this app to life and encourage some sort of interactive interface. In conjunction with JavaScript, Firebase was responsible for the storage of users registered as well as authentication in facilitating logins and logouts.


# Dependencies 
JavaScript npm (10.8.1).  Get it here : https://docs.npmjs.com/downloading-and-installing-node-js-and-npm
Firebase (Google Account)


# Set up Environment

1) Fork this project into a new repository free of any external dependencies. See how here ( https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo)

2) Set up Firebase. (If you never used it before, sign up here https://firebase.google.com)
Login and head to Firebase console, click Add Project (or Create a project), then name your Firebase project. Click through the project creation options. Accept the Firebase terms if prompted. On the Google Analytics screen, click "Don't Enable", we won't need that for such a simple application but feel free to enable it if you need to handle to larger events.

3) Enable email sign-in for Firebase Authentication.
Here, is how we are going to verify and register users who will ne attending. In the left-side panel of the Firebase console, click Build > Authentication. Then click Get Started. Select the 'Sign-in method' tab. Then, click 'Email/Password' from the provider options, toggle the switch to 'Enable', and then click 'Save'.

4) Set up Cloud Firestore.
This is how we are going to set up our database to store registered users. In the left-side panel of the Firebase console, click Build > Firestore Database. Then click 'Create database'. Select the 'Start in test mode' option, this allows you to write to the database during development. Click 'Next', where you will select the location for your database. Default is fine but I usually select the closest zone to my location. Finally select 'Done'.

6) Add and Configure Firebase.
Navigate to your project's overview, by clicking 'Project Overview'in the top left. In the center, select the web icon (</>), and register it with the name you want to name your application. Click 'Register app'. In the Firebase console, click the settings icon in the top-left corner, then select 'Project settings'. In the 'General' tab, scroll down to the Your apps card.
Click on your web app, then select the 'config' option. Copy the entire configuration snippet to your clipboard. Go to "index.js" and replace the current 'firebaseConfig' object with the one saved on your clipboard. This is to configure and link your Firebase project with your web app to streamline your database and authentication services.

7) Set up Firebase Security Rules
We initially set up Cloud Firestore to use test mode, meaning that our database is open for reads and writes. However, test mode should only be used during very early stages of development. In the Firebase console's 'Build' section, click Firestore Database, then select the 'Rules' tab. Delete the existing match "/{document=**}" clause. Then, in the clause "/databases/{database}/documents", identify the collection that you want to secure. (match /guestbook/{entry} {} in our case)

8) Update Security Rules
Since we used the Authentication User ID (AUID) as a field in the guestbook document, we can get it and verify that anyone attempting to write to the document has a matching AUID. Enter this:
'''
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /guestbook/{entry} {
      allow read: if request.auth.uid != null;
      allow create:
        if request.auth.uid == request.resource.data.userId;
        && "name" in request.resource.data
        && "text" in request.resource.data
        && "timestamp" in request.resource.data;
    }
  }
}

'''
9) Compile and Test.

# Credits

Kiana McNellis and Rachel Saunders contribution to Firebase guide on this project.
Can be found here  : https://firebase.google.com/codelabs/firebase-get-to-know-web?continue=https%3A%2F%2Ffirebase.google.com%2Flearn%2Fpathways%2Ffirebase-web%23codelab-https%3A%2F
