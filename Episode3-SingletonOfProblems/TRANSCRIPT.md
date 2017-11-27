This is Segments, an Android developer podcast where we tell real world developer stories. I am Dave your host for today. On this episode of Segments, I will be sharing with you some of the issues you can run into when using a Singleton to store stateful data in an Android app.

This is a common recurring mistake made by lots of developers new to Android. Because it is an easy solution, developers turn to using a Singleton before considering something slightly more complex like a SQLite database.

For example say you say you had a Customer support app that needed need to display a list of frequently asked questions. Where those questions are acquired from a server’s api. Each question when clicked on in     the list displays more information in another activity.

A naive developer may choose a Singleton to store the data retrieved from the server. It’s an easy implementation, and can be accessed easily from anywhere in the app. The first mistake they will probably make though is assume that when the user is at the detail activity that the Singleton data will always be there.

I’ve seen this done many time in apps. For example the list activity would make the network request, store it in a Singleton, and then the detail activity would access that data assuming the  was there for access. At first this seems like a safe assumption for the inexperienced developer. Why? The detail activity is only accessible from the list activity, so how could the user ever be on the detail page with an empty Singleton class.

Well they can be at that page and have no data from the network. How? If your user makes it to the detail activity the first time and backgrounds the app. The Android app remembers the last activity your user was at, and the system will resume the app at that activity.

This is all fine and dandy, as long as your application is kept running in memory. You see that static memory you’ve used to allocate the Singleton lives with the life of your application.

But on the Android system there is a use case you should be prepared for, and from my experience this happens much more than developers give credit for. You see the operating system can stop executing your application at any time when it is in the background. When this happens, if your user resumes your application from their backgrounded apps, they will start again at the activity they left off on.

So it is very possible for the user to restart at that detail activity, just after your application freshly launches. This means any code in the list activity will have not ran. Which means all that static memory in the singleton is not initialized, and boom you have null pointer exceptions.

So what is a better solution? Anytime you have anything you want to persist in a mobile application you want to be looking at a type of persistence that lives beyond the life of your application. While Singletons can be done, you have to be aware of the limitations and be ready to re-initialize it via network requests at any point in your application. This can get complex, and what if your user doesn’t have a network connection, what do you do then?

My rules for persistence are simple, if it’s a small configuration based thing. A user’s email, or a settings preference. Any type of data that will always be a one off, then I uses SharedPreferences. SharedPreferences is actually super easy to work with in Android, and almost even easier than a Singleton, and it gives you the longevity persistence you are looking for.

If the data is something that is repetitive, like a list of objects that represent the domain data for the application, then what you want is a database. I recommend going one of two ways, either using     ROOM a Google SQlite library or using Realm a database solution developed with mobile devices in mind.

Both are easy to get up and running, and they will also help you architect a solid solution for your persistence layer in your application.

Keeping in mind that your application can be stopped and returned to at any point by the system. Even when you have a proper database solution in place you want to test for these use cases.

You can easily have the system destroy parts of your application while navigating it. Just turn on “don’t keep activities” in your developer settings on the device. This will ensure that each activity you leave will be destroyed and recreated as you return to it.

But how do you kill your entire application at a specific activity or point? And you do want to test for this, because there is always some static memory somewhere that can sneak by you if you don’t. I use the Android device monitor. With it you can see all of the running process on an app, and are able to kill any of those processes. All you needed to do was background your app, kill it via the device monitor, and then return to the app from your background apps menu to experience entering your app after it was killed.

You can find the android device monitor via the tools file menu in the Android option in Android Studio.

When I was first starting out as an Android developer I was bitten by this mistake, of using a Singleton to store data from the api. It worked well until I started getting back weird crashes that I could not make sense of in my crash logs.

I thought to myself hmmm there is no way the user can be at that screen and that can be null. Sadly it never made sense to me to till much later. And I ended architecting tons complex code to catch and recover my singletons in these odd states.

I’ve seen this too many times now being done in 3rd party libraries, and other people apps. So I am hoping I can catch some other developers new to Android before they choose what seems to be the easy way to do things.

Setting up an SQlite database used to be a bit of work, but we have tons of great 3rd party libraries to help bring down the boiler code. I’ve personally used DBFlow which is a great library, but even better now, I feel you have no excuse not to, with the introduction of ROOM with the architecture components from Google.

I highly recommend using that and not going any other way. Realm though you are not into SQL relational databases, is a strong competitor as my second recommendation.

At the end of the day it is ok to use Singletons, but they aren’t meant for storing state. A Singleton that some how provides a unique service in your app is still valid, and should be able to be accessible at any point in your app in any state. Though still think twice on using Singletons at all, they can make unit-testing a serious pain.


This has been a Segment. I am Dave Anthony Thomas, and you can reach me @dave_a_thomas on twitter.

Cheers Folks!

