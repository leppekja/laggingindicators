---
title: "2 years of notes from building an app I didn't release"
date: "2024-11-25"
categories:
  - "personal"
tags:
  - "coding"
  - "programming"
  - "project"
  - "technology"
  - "web-development"
aliases:
  - ../../2024/11/24/2-years-of-notes-from-building-an-app-i-didnt-release
---

Close to 2 years ago, I had an idea for a mobile application that allowed users to store, submit, and manage tax reimbursements in foreign countries. For 2 years ago, I on and off worked on this idea and had a functioning version on my phone - and for nearly all that time, I never felt like the application was ready to release to potential users - and it doesn't look like I'll ever get it there.

What follows are my work logs for the past few years from building this application, and what I learned.

### May 2023 Choosing a framework

Since I know users use iPhones for their work, and I have an Android device, I wanted a cross-platform framework so I only had to manage one codebase and learn a single new language. I considered building just a web app, but wanted the experience of downloading the app from the app store, and learning how to do it myself.

I had some experience with React, so I first looked up React Native and started trying to install that. Exploring on the web led me to Flutter and I liked the fact it almost included a framework for the UI. Less work for me, and it was nice that it would align with usability needs and look professional and familiar to users.

The deciding factor was that I managed to get Flutter installed and working first before I got React Native working on my machine.

### June 2023

I didn't start documenting my work process immediately when building the app. In June, and May before it, I worked on the UI of the app, specifically:

- Built a mock home page

- Built a mock cedula card page (user information)

- Enabled the taking of pictures

- Enabled a list page of receipts

- Created a mock data model and allowed for local storage

It was at this point when I needed to go further and implement a database that I started writing. Local storage didn't seem like a necessarily secure or long lasting model. Families would want to share receipts across purchases, and access them on different devices to download and print. I would need to upload the receipts anyway to APIs to OCR the submission instructions, receipt IDs, and other purchase information.

#### Flutterfire & Firebase

Incorporating authentication, analytics, and a database into the application.

I decided with Firebase for a few reasons:

- I didn't want to worry about platform-specific APIs that much

- Wanted something pre-built that I could outsource authentication, security, etc to

- Saw it had online and offline capabilities, which I thought was nice.

- Seemed to have good support with Flutter, and a decent amount of code-labs and instructions at first glance. I looked at AWS (Amplify?) and Azure as well, but since I was already using a Google framework, I decided the path of least resistance was likely a Google back end as well.

#### Organization Name

I had built my app in the sample directory Flutter created as part of a code lab, which mean the Organization ID was com.example.something, which is disallowed by the App Store or something else. So at this point I created a new Flutter project, then copied my code over.

I also refactored here to split up all the code in `main.dart` to specific files for the camera, profile, and receipt section.

Here, Flutter notified me that Flutter had updated and I needed to run `flutter update`.

On trying to build my app after this, I had to restart VSCode because the `Future` type was not being recognized by Dart. Restarting fixed that error.

Now, Flutter told me to run `dart fix` to fix common issues in my files (apparently which happens after a Flutter update). I did, and it returned help text. I needed to run `dart fix --apply` to have the fixes implemented.

Here, I tried again to run `flutterfire configure --project=PROJECTNAME`. That returned a `pubspec.yml` not found in `flutterfire/cache/something` error. I re-logged into Firebase, then re-ran `dart pub global activate flutterfire_cli`. This activated `flutterfire` again, after which running the `flutterfire configure` command worked.

I had to re-run `flutter pub add firebase_core`.

I keep getting a

`Building with plugins requires symlink support. Please enable Developer Mode in your system settings. Run start ms-settings:developers to open settings.`

error. No idea what `symlink` is at all.

At this point, the imports fore firebase_core and firebase_options are no longer marked as errors, but `FirebaseDatabase` class still is tagged as undefined. I thought it was an error and restarted VSCode, then I realized I didn't actually bring over the import for the Firebase RealTime database because I decided to implement the basic authentication module first. My bad.

I ran `dart fix` again. I don't know why. I thought I just ran it. But it asked me too, so that's that.

### Authentication

I followed [this article](https://firebase.google.com/docs/auth/flutter/start?hl=en&authuser=0), and the this [codelab](https://firebase.google.com/codelabs/firebase-auth-in-flutter-apps#2). Something I like about Flutter so far is the simplicity of installing and adding packages.

I ran into an issue in Step 5, Sign-In screen, when trying to add in the `headerBuilder` option. VSCode (or me) had set `const SignInScreen` to return, rather than just `SignInScreen`. When I got the the `headerBuilder` arg, I kept getting an `Invalid const` error, which I kept thinking was the `headerBuilder` since that is what was being highlighted. But I had set the whole widget to a `const`, and the `headerBuilder` couldn't be calculated as a `const`.

I had to set the `assets: - images/` folder in the `pubspec.yaml` folder for the `SignInScreen` widget to display the icon correctly. I also updated the size and background and added a second, different sized version to show on this page, versus the launcher icon.

### Icon

I designed a simple icon in Figma (free plan) in about 15 minutes and saved as a .png. It took a bit to find the actual developer guidelines on how to build the application launcher app. I was also confused that I had to install `flutter_launcher_icons` to update the application icon, and I didn't find the documentation very helpful. I copy-pasted the configuration example from the docs page. I ran `pub run flutter_launcher_icons`, which, I don't exactly know what it does.

The icon successfully updated, but when the app loads, the icon is magnified, and the image I uploaded isn't (so it looks small in comparison).

I don't know if it worked for iPhone, or if I need to upload multiple sizes. I got a "Overwriting default iOS launcher icon with new icon - No platform provided" note when I ran `pub run flutter_launcher_icons`.

### July 2023 Profile Page

**Restarting the project**

I had been away on vacation for 2 weeks. I hadn't worked on this app for probably 1 to 2 weeks before that, as well, due to wrapping everything else up before the vacation. This is rough, because it is hard to keep track of a) what I did last, b) if I left it in a state where I need to do something crucial next that I don't remember, and c) what I planned to do next. So, my steps to restarting a project now:

1. Try to run the project. See what happens.

2. Run anything that the IDE asks you to run. E.g., `dart fix`. I swear I get this message every time I open VSCode.

3. Focus on the part you think you remember doing last and spend more time understanding that section than anything else.

4. Make a minor, completely unrelated change that is almost worthless, e.g., adjust the padding on something, just to see if you remember how to commit a change. This will also feel like quick progress.

I previously think I was working on the Google Firebase integration, so restarting work there first. I also set a goal. By the end of July, have a publishable app that allows users to log in and store their organization tax information. This is the information that the store will ask you for when you try to generate a reimbursment. I already have the major parts of this, but I want to polish it and ensure it works, and publish it (at least informally).

I read about logging options due to a warning that tells me I shouldn't use `print` in production code. That said, all the examples I found online about Dart logging literally just printed warnings from the `logging` package to the standard output, and ended there, or just used `print` statements themselves. So not worth it at this point for me to fix.

### Firebase (Realtime Database)

I started with the Firebase documentation and quickly felt overwhelmed. I had not worked with a JSON tree type data structure before. I read the [Firebase Flutter articles on Realtime Database](https://firebase.google.com/docs/database/flutter/start). I can't say I deeply understood the [Choose a Database](https://firebase.google.com/docs/database/rtdb-vs-firestore) article either, but I assumed that for this minimal app, either would work. So, I went for the simplest.

I ran `flutter pub add firebase_database` and `flutter run`, and added the Realtime Database package to my `main.dart` file. I was looking for a simple example on a) how to set up the data structure and b) how to write and read data, which incidentally are two of the five help articles they had (at least at the time of writing). Neither, however, were in-depth enough to get me started.

I switched to [a Google Codelab](https://firebase.google.com/codelabs/firebase-get-to-know-flutter) which covered how to add Firestore, rather than the RealTime database, but close enough.

I ran into this error:

`: Error: Too many positional arguments: 1 allowed, but 2 found firebase_auth_web.dart:94 Try removing the extra positional arguments.`

`FirebaseCoreWeb.registerService('auth', (firebaseApp) async { ^ : Context: Found this candidate, but the arguments don't match    firebase_core_web.dart:43static void registerService(                ^^^^^^^^^^^^^^^    Failed to compile application.`

Fortunately apparently this was a very recent issue for many people and resolved it with [this StackOverflow post](https://stackoverflow.com/questions/76500308/flutter-web-fails-with-firebase-too-many-positional-arguments-1-allowed-but-2).

I followed this tutorial for a bit, implementing the UI for it. Then got distracted with how the screen looks. I implemented some UI changes, considered how I wanted the profile to function, and explored some example applications. I decided that editing the information would be done on a separate screen, to allow for users to click on each piece of information and immediately copy it to their clipboard. I then implemented the copy to clipboard functionality, which happily, was a trivial task.

Here, a couple things to do (to keep track for myself):

- refactor the label and profile text to be a container, so you can click anywhere on the text or label to copy.

- Implement a copy all functionality.

- Implement a edit page (imagining a pencil icon leading to a new page)

- Actually stop procrastinating on the difficult problem and implement the database.

### 7/18/2023

I watched [this Firecast video](https://www.youtube.com/watch?v=EXp0gq9kGxI) that helped me ensure I had the packages installed correctly. The last step failed for me when trying to compile to Chrome Web, as I am using VSCode and didn't have the `logcat` window. I couldn't find an equivalent debug console, and thought that perhaps I didn't properly connect the Web version to the Realtime Database. So I turned on an emulater (Pixel 2 API). From this, while compiling, I received a `...FlutterFirebaseFirestoreMessageCodec.java uses unchecked or unsafe operations. Note: Recompile with -Xlint:unchecked for details.` error. Found [this StackOverflow post](https://stackoverflow.com/questions/76145595/flutterfirebasefirestoremessagecodec-java-uses-unchecked-or-unsafe-operations-n).

Before updating the SDK version, I tried the simple database call laid out in the video. Finally, a `Caused by: com.google.firebase.database.DatabaseException: Firebase Database error: Permission denied`, which was good news. At least it was doing something.

Looking in the console, my Rules prevented access.

`{"rules": {".read": false,".write": false}`

Based on reading the [Firebase security rules documentation](https://firebase.google.com/docs/database/security/rules-conditions), and a quick query to Github Copilot, I adjusted these to:

`{"rules": {"users": {"$user_id": {".read": false,".write": "$user_id === auth.uid"}}}}`

Of course, for some reason I can't tell, hot restart isn't working. I have to manually click the hot restart button for the app to reload.

But now it works! I had the update the code in my `onPressed` call as well to get the unique ID of a user I had previously created. It now looks like this:

```
    onPressed: () {
        FirebaseAuth _auth = FirebaseAuth.instance;
        User? user = _auth.currentUser;
        String? uid = user!.uid;
        DatabaseReference _testRef =
            FirebaseDatabase.instance.ref().child('users/' + uid);
        _testRef.set('Hello World');
    }
```

Which was a slight adjustment from the installation video linked above. I also manually change the JSON data path of the Realtime Database under the Data tab in the Firebase Console. Where it previously stated `https://project_name.firebaseio.com/`, I added a key `users` and another object below with the user ID of the user I had previously created. I looked this up in the Authentication Console and copy-pasted. The data model now looked like:

```
    https://project_name.firebaseio.com/
    - users
        - user_id:""
```

After restarting my project and clicking the button with the function above, it added "Hello World" as the value for the `user_id` key. This was a big accomplishment! I now have a database.

Here, I manually entered sample data for the profile page for this user. One thing that confuses me at this point is how to create the data model; if that is manually done from the console? How do I add a new `user_id` branch in the `users` branch, for example? So I pushed passed this, and for the rest of today, just wanted to get a working example of reading data from the database for the current user. I updated the `read` rules on the `users` and new `profile` branches to all be `"$user_id === auth.uid"`.

Here, I returned to the [in-depth video of incorporating RealTime Database into Flutter](https://firebase.google.com/docs/database/security/rules-conditions). And with a bit of refactoring, including adjusting my \`ProfilePage\` widget to a Stateful Widget from a Stateless widget, and copying the code from the video, I had a working event listener set up that was reading data from the database.

The video was excellent, detailed, and got it working. However, it did that thing that teaches do where they teach you the less good method first, get you to do it, then tell you about a different method that is much better in every way. So now I apparently need to delete a bunch of that code and forget about that event listening process and use a StreamBuilder instead.

### 7/28/2023

It has been 10 days since I've last looked at this. I quickly need to remind myself how to open Visual Studio Code and what all this multi-colored text on my screen is. I see a triangle with an exclamation mark inside, and a bright blue bubble with "33" inside of it. I literally have to scroll to read all of the warnings. I either make actual progress on my app, or add a `const` keyword in 30 different places for the next 20 minutes.

I remember that at the end of the last day, everything broke and I spent a couple hours getting progressively angrier and angrier due to a `Map<object, object> can't be assigned to the parameter type 'Map<String, dynamic>'` or some similar error. Turns out the answer was in the Youtube comments section, and I needed to cast the returned value to a `Map<dynamic, dynamic>`, rather than a `Map<String, dynamic>`, which I am unsure why still. But it works.

Another error I ran into here was a `LateError (LateInitializationError: Field '_myStream' has not been initialized.)`. If I tried to leave the Profile page, the app would freeze and throw this error. Looking at the call stack (which I am only now learning how to use this properly), I see it pauses on the `deactivate` method of the Profile page I am setting up. I rewatched the video (around 23 minutes) discussing the set up and take down of the `StreamSubscriptio`n. For some reason, I had `final _myStream` assigned to the event listener. Removing `final` solved the problem. Again, I don't understand why at all. `late` tells the compiler (I thought) that later, I would assign the `StreamSubscription` to a value, which I did to the event listener.

The `final` keyword tells the compiler that the value of the variable won't be changed later, so perhaps this is incompatible with the event listener, which could change in the future? Or perhaps it can't be cancelled if the value can't be deleted? That is my guess, at least.

I also spent a half hour (trying) finishing setting up Google Analytics in the application, based on [a tutorial here from Firebase](https://firebase.google.com/docs/flutter/setup?platform=ios). I don't care much for these tutorials where the tutorial is the installation, then a code file that you are supposed to add to your project. That isn't a tutorial - it's a code file. But I faithfully copy-pasted the code from the example `main.dart` into my existing project. The [Google Analytics get started tutorial](https://firebase.flutter.dev/docs/analytics/get-started/) ends at `FirebaseAnalytics analytics = FirebaseAnalytics.instance;`. And there is no more, even though apparently that is not enough to get it working. Or perhaps it is. It isn't clear how quickly the events will start being logged to Google Analytics, so maybe I just need to be less impatient.

I tried to enable Debug mode by running `adb shell setprop debug.firebase.analytics.app PACKAGE_NAME` in my terminal, as [suggested here in the DebugView tutorial](https://firebase.google.com/docs/analytics/debugview). I am using a Pixel emulator right now, as my phone USB-C connection is not working right now for some reason and refusing to connect to my laptop. That, unsurprisingly, did absolutely nothing that I could tell.

Here, I also connected the rest of the Profile fields to the event listener approach (saving the update to StreamBuilder for another day). I also added a `persistentFooterButton` to the Profile page, allowing a user to copy all the fields to insert into a text message or email (common action for the purpose of the application).

Next Steps:

- Edit profile fields

- Reorganize so MVP of app is just the profile card and nothing else

- Understand log-in flow (how to add new users?)

- add profile fields

### 8/5/2023

It is midway through projects that the scale of really releasing a production application starts to intimidate me. It is not difficult to start a project like this; the app developers and learning resources all aim to get you started. The novelty of getting something on the screen wears off. The screen is suddenly superficial as I realize the depth of what yet needs to be implemented, a majority of which is totally invisible to the user. Looking at the application now, it doesn't even feel like I am building anything. There is nothing I can touch or hold on to. Nothing feels like I've created it. Instead, it is word after word, click after click, all dependent on machinery I don't understand. I don't feel like I have made anything new - I'm only implementing something that exists, already, that is doable by anyone.

### 8/20/2023

Pain in my wrists, so I haven't been working on this project at all. So I am simplifying - version one, only display card information, and that's it. See if people even use it, and if they do, then I'll keep working on it.

### 8/22/2023

Checking to see how to create a new user and create that sign-up flow. But it appears Firebase Auth has that built in! Amazing. Successfully created a new user, but didn't handle getting returned `null` data from the Realtime Database for a new user for the new home page, so had to resolve that. First had a `Unhandled Exception: type 'Null' is not a subtype of type 'Map<dynamic, dynamic>'` error, which I resolved by adding an if-then statement to check if the data is null prior to creating my object from the data.

I leveraged GitHub Copilot here. Did a great job explaining to me the difference between `event.snapshot.exists` and just a if-then checking if `event.snapshot.value` is `null`. The `.exists` method checks if the reference exists or not, regardless of whether it is null. One aspect I find valuable is Copilot's attempts to provide an example based in the code I am currently working on. This is usually where it goes wrong, sometimes getting the logical flow of the code incorrect, but good enough that I can see it and adjust. It also reminded me how to properly write an if-then statement in Dart and how to resolve the `local variable not used` error that came up.

### 9/18/2023

Another long break away from the application. Severe forearm pain and nerve issues in my leg are making me wary of being on a computer for too long. Family emergencies, weddings, and dinners with friends fill up the time.

But I'm close. I start today with the aim of getting a About page in the application, and fixing the App Bar. I move the copy button to a floating action button, fix the color on the log out button. I add an App Drawer as an expanding side menu. I'm increasingly turning to Copilot to answer my questions and help write boilerplate code rather than StackOverflow.

I update the user information in the drawer. Null checks continue to be difficult to get used too. What if the user doesn't have a name? I need to use the email instead. I spend a half hour trying to get the first two characters of either, but my `substring` call keeps telling me `RangeError... :Invalid value`, which confuses me, since one of the strings is longer than 2 characters and I have a `a ?? b` null check.

Finally I solve it with a ternary condition. `a` wasn't null, but an empty string. Now to the actual problem of where each page takes the person. I'd like to include details about the the filing process for my users. Right now, it's just a card on the phone that a user could substitute a picture of the card of for most cases. Sure, the copy functionality might save someone a few minutes, but not enough. If I can provide answers to some commonly asked questions, to serve as a sort of reference document, then that could be another low-hanging fruit to attract users to log in occasionally. I'm not sure where to get the information though, or how to keep it updated -> I don't want to give wrong information, also. One wrong answer and I imagine people will be wary of future information, especially early on.

I create an About page that includes an overview of the app, upcoming potential features, contact information, and a simple privacy statement.

### 10/29/2023

Another long break in development. I'm not even sure what I need to do. I started with adjusting the copy area in the profile section, so clicking on the section name or the value copies the text to the clipboard, rather than just the value, making it a bit easier to get the values.

But I really need to work on the Edit profile page.

I started off with replacing the separate Edit page with a callback function in the App Bar that flips a `_isEditState` toggle, switching the widget from a text display with a copy function to a `Form` based off of [this tutorial](https://docs.flutter.dev/cookbook/forms/validation).

### 10/29/2024

The last time I worked on this app was 1 year ago exactly. I only realized this as I opened the project again. I didn't realize it had been this long of a break, and I am disappointed in myself for not getting to this sooner. However, I have a clear goal - to take screenshots of the app to share with people to see if they would be interested in using the application.

That said, I have been using the app consistently during the year, and at checkouts, restaurants, and at home the test version of the application has been consistently helpful. I have mentioned it to other people who all expressed interest in something like this, and I am still motivated to finish this with a release.

I started by trying to run the application in an emulator. The debugger would start, run `assembleGradle`, then fail. I found the `flutter daemon` logs after pressing the run and debug button with multiple configurations, which stated a few errors:

```
> [ERR] Command exited with code 128: git fetch --tags
> Standard error: fatal: unable to access 'https://github.com/flutter/flutter.git/': Could not resolve host: github.com
> WARNING: your installation of Flutter is 531 days old.
> To update to the latest version, run "flutter upgrade".
> Starting device daemon...> [ERR] Error 1 retrieving device properties for sdk gphone... x86 64:
> [ERR] adb.exe: device 'emulator...' not found
```

Some of these I could handle, and I ran `flutter upgrade` to get this out of the way. This of course also returned an error.

```
> Your flutter checkout has local changes that
> would be erased by upgrading. If you want to keep
> these changes, it is recommended that you stash  
> them via "git stash" or else commit the changes  
> to a local branch. If it is okay to remove local
> changes, then re-run this command with "--force".
```

I committed and pushed all files I had updated locally, not realizing this wasn't exactly what the error was pointing me to - that apparently I had changed Flutter itself? I confirmed this with a web search, [finding a StackOverflow answer](https://stackoverflow.com/questions/73380891/your-flutter-checkout-has-local-changes-that-would-be-erased-by-upgrading-whe) and - after looking for the directory that I installed Flutter to - I ran the suggested command: `git diff HEAD`, which returned nothing. I had tried to run the `flutter upgrade` in my local project directory; apparently this was wrong and I should run it in the installation directory, and doing this returned the same local changes error. Running `flutter doctor` showed no issues in my installation.

It was running `git status` in the installation directory that showed me the following message:

```
> On branch stable
> Your branch and 'origin/stable' have diverged,
> and have 20 and 7227 different commits each, respectively.
> (use "git pull" if you want to integrate the remote branch with yours)Untracked files:(use "git add <file>..." to include in what will be committed)
.pub-cache/
```

I ran `git pull`, which resulted in many merge conflicts. I tried running `git status` once more, which returned now a massive list of updates and modifications. Trying to cut my losses, I ran `flutter upgrade --force`, which came back with a `<< was unexpected at this time`, which was also surprising to me. Another web search got me to just resetting the HEAD, to remove perhaps the changes that got included in the `git pull` command I tried? I then ran `flutter upgrate`, a typo, but it did start running `flutter upgrade` anyway. Still, I cancelled, ran `flutter upgrade`, and this started the process working normally, then errors with the same local changes error above. I am unsure what changed exactly in my `.pub-cache/` file, but a year after last touching the project, I don't imagine it will be relevant. I simply ran the upgrade with the `--force` command. During this, VSCode prompted me to reload Dart, after the installation was complete, and since my terminal is on the right hand side of my screen, the VSCode message hides any text at the bottom of the terminal - and so I thought the process was complete, clicked reload, and potentially interupted the `flutter upgrade` process. But it still reloaded the Flutter SDK, and then VSCode again prompted me to run `flutter pub upgrade`, so sure, great, I ran that too.

Another message at the end:

```
> Building with plugins requires symlink support. Please enable Developer Mode in your system settings. Run
> start ms-settings:developers
> to open settings.
> exit code 1
```

So I enabled Developer Mode, and tried to run `flutter pub upgrade` again, but still in the Flutter installation directory, which gave a `expected to find project root in current working directory` error, so I guess I need to run this in the actual app directory. Couple of `cd ../`s later and it tells me "no dependencies changed,", so that doesn't make sense given it provided a list to update, and a few packages have apparently been discontinued, like `firebase_dynamic_links`. The terminal suggests `flutter pub upgrade --major-versions`, and what the terminal suggests, the terminal gets.

I moved on, and just ran Run and Debug from `main.dart`, laughed at the "Errors exist in your project" message, and clicked run.

Close, but still failure.

First I get this error:

```
> You are applying Flutter's main Gradle plugin imperatively using the apply script method, which is deprecated and will be removed in a future release. Migrate to applying Gradle plugins with the declarative plugins block: https://flutter.dev/to/flutter-gradle-plugin-apply
```

Deprecation means it still works, so can ignore that one. The key error (hopefully) here:

```
> - Where:
>   Script 'C:\Flutter\flutter\packages\flutter_tools\gradle\src\main\kotlin\dependency_version_checker.gradle.kts' line: 375
- What went wrong:  Failed to apply plugin class 'Dependency_version_checker_gradle$FlutterDependencyCheckerPlugin'.
> Error: Your project's Kotlin version (1.6.10) is lower than Flutter's minimum supported version of 1.7.0. Please upgrade your Kotlin version.
> Alternatively, use the flag "--android-skip-build-dependency-validation" to bypass this check.
```

Great! Just upgrade a whole different library. But this error was informative enough and even as Android Studio booted up, I just changed my `build.gradle` file to point to version 1.7.0.

Aside from an error in my incomplete edit profile widget, which I commented out for now, I encountered multiple errors with `TextTheme` colors not being defined. Perhaps this was the `.pub_cache` error from earlier, and I [found another StackOverflow question discussing the upgrade error](https://stackoverflow.com/questions/76015716/the-getter-accentcolor-isnt-defined-for-the-class-themedata), and the [Flutter documentation](https://docs.flutter.dev/release/breaking-changes/theme-data-accent-properties). I ran `flutter pub cache clean`, given that the colors error were coming from `material.dart` in the cache folder, referencing the `flutterfire-ui` package, and followed up with `flutter pub get`, as the terminal told me to do.

Additionally, `flutterfire-ui` apparently was deprecated and broken out to a different repository, so I had to update `auth.dart` and `main.dart`. It isn't clear from the new example in the changelog to update `EmailProvider()` to `EmailAuthProvider`. Additionally, this is the first time I've seen the `hide` option in an import used, such as:

```
import 'package:firebase_auth/firebase_auth.dart' hide EmailAuthProvider;
import 'package:firebase_ui_auth/firebase_ui_auth.dart';
```

I finally came across [the exact issue in the repository](https://github.com/firebase/FirebaseUI-Flutter/issues/236). One comment suggests updating the SDK environment versions, but I don't know how to find what the current versions should be. I simply updated the upper boundary to the version I saw listed on the repository, but didn't know what to switch the lower boundary to. I tried manually updating some of the firebase_auth_ui packages, which simply resulted in dependency conflicts and the automatic version solving erroring.

I ran `flutter pub upgrade --major-versions`. Trying a build here gave me `The plugin firebase_auth requires a higher Android SDK version.`, so I needed to update to `minSdkVersion 23` in the build.gradle file. It notes that "Following this change, your app will not be available to users running Android SDKs below 23," so tough luck to those folks. I had to do this twice, admittedly, since I added the whole suggested block to the `build.gradle` file in the `/android` directory, when I needed to update the `build.gradle` file in the `/android/app/` directory.

And this did it! The emulator started up with the latest version of the application. Only took multiple hours, and it was a bit of a frustrating experience. All this for a few static screenshots for promotion.

Finally to actual coding. Estimated time spent today on this is probably 4 hours already. I re-add in screens I had previously taken out to add screenshots.

My initial outreach to potential users will just be a WhatsApp message to the target user group, asking for test users, accompanied by a brief overview of key functionality and a screenshots.

I also made a Google Play Developer account, alongside a new email address for the app, paid the $25 developer fee, and completed the identity verification. I had not realized that to launch, you needed at least 20 test users for a 2-week period.

### **11/6/2024**

Have been thinking about this project daily. Wrote up a sample feature set for a version 1, 2, and 3 in my Notes application at about 3 AM one night in the past week. I have been considering an initial switch for the test launch to store data on the user's phone locally, to reduce initial latency when logging in, plus to allow offline usage. Offline usage is probably the key feature, especially usage overseas or within buildings with poor cell reception.

I tried some YouTube videos about this on SQFlite, Hive, Shared Preferences, and Secure Storage. All of these primarily to me seemed like additional work - Shared Preferences wasn't suitable, and SQFlite and Hive seemed like overkill for just initial storing of simple key values. Secure Storage seems like the best option. Prior to taking any action, I just reviewed Firebase to see what I could learn about existing latency and caching options, and started to install the performance monitoring tools available - but it's late, so I'll continue tomorrow.

### 11/7/2024

Continuing to try to install the performance library for Firebase. Had to reinstall the `flutterfire_cli`; no idea why, but I did. Running `flutterfire configure` after installing the library fails.

```
> FirebaseCommandException: An error occured on the Firebase CLI when attempting to run a command.
> COMMAND: firebase projects:list --json > ERROR: Failed to list Firebase projects. See firebase-debug.log for more info.
```

In the log file:

```
> [debug] [2024-11-08T01:44:31.164Z] FirebaseError: HTTP Error: 401, Request had invalid authentication credentials. Expected OAuth 2 access token, login cookie or other valid authentication credential. See https://developers.google.com/identity/sign-in/web/devconsole-project.
```

Running `firebase login` in the console tells me I am properly logged in. Clicking on the link in the log redirects to https://developers.google.com/identity/sign-in/web/sign-in, which seems to be an entirely different topic under a "Deprecated" section, so that's great. After trying to restart this application development after a year, I think that stability is an aspect of any next framework that I would look for before starting.

DuckDuckGo and [StackOverflow](https://stackoverflow.com/questions/52891500/http-error-401-while-setting-up-firebase-cloud-functions-for-android-project#52891586) solved this in website result 1; it was Copilot's 3rd suggestion, so logging in and out of Firebase solved it. Curiously logging out gave a `Invalid refresh token for <email>, did not need to deauthorize` message, but not sure what that means, and I don't care enough to explore. But `configure` finally ran.

### 11/8/2024

First morning session. An hour a day is my goal, and the key last improvements I want to make is improving the receipt details page to make it look better. I was thinking that at this point, I should try to stay as close to the default formatting and constructions as possible to make it as easy as possible. I want to reach presentable and no more, so while I can look at a page and make endless small improvements on it, it looks good enough when I stop that a person could see how they use it.

I did add some small additional information and indicators that would help users see what actions they could take from a screen shot. For example, a button and a disabled button, indicating that actions vary on what can be done, and colored indicators based on data. In my mind, I want people to be able to reach the conclusion that the app automatically reacts to the data stored, and customizes their interactions with each object based on its attributes. On green and red checkboxes, I added "(eligible)" or "(not eligible)" to make it clear what the colors indicated, for example, even if this wouldn't be necessary for a daily user in the future.

### 11/12/2024

Anxiety is high before launch, and I am struggling to decide between approaches. A casual text message with screenshots asking people to reach out with their information for a test group, or build a professional landing page to obtain emails?

### 11/24/2024

A few weeks break again due to family events. And this time, I am deciding not to release the application.

As of now, I would no longer consider myself a "user" of the application, which, to me, is the primary driver of enjoyment when building the application. If I am no longer building it for myself, then, suddenly the application is now a burden. Maintenance, updates, security - not to mention the costs of any database, developer accounts, or APIs I would rely on. Right now, adding another 10 hours a week where I need to sit in front of my computer and code isn't something that I am looking for. I think I was naive to think that "finishing" a project like this is releasing it - releasing it is starting the project.

I don't think this is a permanent decision - mid next year, I may need this application again, and if that is the case, I wouldn't doubt that I would enjoy diving back in and building it - but to the extent that I would find value in it.

This said, I am quite proud of what I have learned, implemented, and and designed. I learned a new programming language and framework and produced a working mobile app that solved a real-life pain point for myself. I have a much greater appreciation for the individuals who do build and maintain mobile apps and open-source programs by themselves now, surely. I don't feel like this is a failure - I didn't approach this from the viewpoint of making money, or a dream of users - but of identifying a problem and fixing it, and that I did.
