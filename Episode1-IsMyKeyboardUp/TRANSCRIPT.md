This is Segments an Android Developer Podcast, where we talk about real 
world Developer experiences.

I’m Dave your host for today, being the first episode of Segments, 
I’d like to take a quick moment to introduce myself and the Podcast. 

I’m a newer Android Developer, who’s been working with the platform since 2013. 
I first became introduced to Android when I helped develop the Android client 
app for ChowNow, an online food ordering system. I am currently working with 
autoTrader.ca where I help maintain the Android Dealer app experience. I was 
also recently involved in the development of MDCalc, an Android application 
for medical calculators. 

Working full-time as an Android developer for the past year, I have fallen in
love with the Android platform. I have about as much iOS experience as I do in
Android, and developing for Android is hands down the better of the two in my 
opinion. Better tooling, an awesome community, and easier and more open APIs 
to work with.

My idea here for Segments, is to tell stories of everyday experiences and 
problems I and other people have had while developing for the Android platform. 
For example, I’d like to share problems that I’ve come up against when using a 
Singleton to store session data, or the things that I’ve learned navigating an 
SQLite database in an app. How to debug that database, and pull it down from 
the device.

Interesting and possibly useful anecdotes of real dev experiences with Android. 
And always, if you the listener, notice something incorrect said while listening 
to the solutions and problems, please reach out and share your knowledge and opinions!

I was also thinking I’d like to interview other everyday Android developers like 
myself. So if you would like to be interviewed on this show do not hesitate to 
drop me a line. You most definitely do not need to be a well known or accomplished 
developer to be on this show. If you’ve only been developing Android for 3 months, 
or you are still just a student, your experiences are just as real and just as important. 

We are all beginners, and we should stay that for life. Humility and humbleness 
are the most important pieces of being a good programmer. Everyday you learn, 
and grow. Everyday you do something new, and with every mistake you make or 
problem you are faced with there is another opportunity there for you to learn more. 

So come on the show! The more you highlight those mistakes, and share them, 
the more you will learn from them, and the more you will grow. 

As a programmer to other programmers, I say never let yourself feel inadequate. 
You are a sponge, and that sponge is your strongest toolset, more so than your IDE.
If you’d like to reach me, I am Dave Anthony Thomas, and I can found at 
@dave_a_thomas on twitter. That was dave _ a _ thomas which is spelled 
t h o m a s on twitter.

In this episode of Segments, I will be sharing with you a story about 
working with the android software keyboard. This story centers around 
he need to know when the software keyboard is shown. Unfortunately for 
Android developers there is no way to know this naturally. 

There is no API method that we can attach a callback to, to receive a 
notification that the keyboard came up. Unlike those lucky buggers on iOS who 
can just goto the Singleton NSNotificationCenter and add an observer for the 
event UIKeyboardDidShowNotification.

Nope, I’m sorry my Android friends, Android does not do things this way. You
see the reasoning is this. Let me quote a StackOverflow post from a popular 
and wise Android developer, whose alias is “CommonsWare” Mark Murphy.

Not all Android devices use virtual keyboards. Some have physical keyboards. 
Since you need to support both types of devices, you need to come up with a 
UI design that does not assume that everyone has a virtual keyboard.

That is some wise advice my friends. 

So in hindsight, if you were to attach a certain behaviour that would occur 
when the keyboard came up, then there is good chance you are alienating a 
group of user’s devices from that behaviour. And that’s not really something 
we want to do as a mobile developer, alienating a certain group of users.

Now what about that one special use case, where you actually need your UI 
to change when the software keyboard becomes visible. Well once we come 
full circle I am going to argue against that actually being a problem that 
we need to solve, but if a curious developer is looking for a solution they 
eventually will find the ViewTreeObserver solution on StackOverflow. 
It works, but it is a hack.

I am going to take us back in time now. To my first Android developer gig. 
I was working for two reputable designers at the time. UI was everything to them. 
To the point where everything we were doing in the app, we were building, was custom.

Being a new Android developer I got my feet wet quickly as I jumped into in the 
deep end of custom views. Lots and lots of custom UI work.

We had one screen specifically that started my journey into researching keyboard 
listeners on Android. It was sign up screen. Simple one, email, name, and password. 
It had it’s special custom look and feel. Nothing like the default Android UI. 

On Android when you focus an EditText, the keyboard comes up and if the mode is 
adjustPan the EditText is scrolled to just above the keyboard. This is mode that 
you will get by default, and this can specified per Activity.

All good I figured that works, the user can fill out one field, press the next 
button and the focus will transfer to the next edit text which then scrolls into view.

Well the UX guys, my bosses, didn’t like this. They wanted all of the fields 
visible while the keyboard was up, so the user could quickly switch between 
the fields. All-right, so I switched to resize mode. 

In Android you can change the way the screen adjusts itself to the keyboard 
coming up by changing the value of windowSoftInputMode. You do this in your 
AndroidManifest. The values available are either adjustPan, adjustResize, 
adjustNothing, or adjustUnspecified. You change this value as an attribute 
on the Activity decleration relating to the Activity you want that specific 
keyboard mode behaviour to occur in. 

I find the Android documentation a little misleading and opinionated here 
when it comes to the description of each of these modes. And here we are 
in 2017…. This was 2013 when I first read the documentation and it looks 
like it still hasn’t changed.

Here is what is what documentation.

For 			It says
"adjustResize"
The activity's main window is always resized to make room for the soft keyboard on screen.

For		It syas
"adjustPan"
The activity's main window is not resized to make room for the soft keyboard. Rather, 
the contents of the window are automatically panned so that the current focus is 
never obscured by the keyboard and users can always see what they are typing. 
This is generally less desirable than resizing, because the user may need to 
close the soft keyboard to get at and interact with obscured parts of the window.

So for adjustResize, the Android documentation recommends that this is the input 
mode we want to use. And for adjustPan, it is a less desirable solution that will 
allow the software keyboard to obstruct parts of your UI.

Actually I’d argue from my experience adjustResize isn’t more desirable. 
From my experience it has been less desirable in the cases I’ve used it.

Because if your view isn’t scrollable it literally just gets smaller in adjustResize, 
so everything gets squished together. And even when it is scrollable adjusting 
a one page UI into a scroll view is a challenge, and I find I usually still 
end up with what feels like a squished UI with in a scroll view.

So here we are adjustResize is giving me what I want it is keeping all the 
views visible but now I have new problems. The logo, and the terms and condition 
text on the screen are all getting squished into my username and password fields.
So I needed some kind of way to know that the keyboard is up so I can hide all of
those unnecessary decorating elements. Off I go to old faithful, StackOverflow. 
It didn’t take much research to find the great keyboard discussion.

You’ll even find a link through an answer on StackOverflow to a google group 
discussion where Dianne Hackborn herself refutes the necessity of such a 
listener. Here is what Dianne has to say;

Why do you want to know it is shown?  The IME being shown has little meaning, 
since exactly how the IME behaves is up to it -- it may be a transparent overlay 
and not impact the application, a small strip, or all other kinds of things.

Due to this, the main way you interact with the IME is by setting your softInputMode 
to be resizeable so when the IME says it wants to occlude part of the screen your 
app's UI will get resized to take that into account if needed.

Dianne was an engineer involved in the creation of the Android framework.. 
An episode I listened to from the podcast Android Developers Backstage where
they were talking about understanding the Android source code references her, 
mentioning something along the lines of “If you want to understand the Android
source code, you need Dianne” The episode is Episode 77: Android Internals with
Effie Barak

So there you have it from a reputable source with in Google, with the Android 
platform you do not need to know when the keyboard is obstructing the screen.

But people have found a way, and there are many implementations of this same 
solution on StackOverflow. And it is hacky. I’ve been bitten by this hack. 
So I warn you, if you stop this podcast now and just use this solution you 
could be making a mistake.

Let me run you through the solution:

First you get a reference to the most root in your current layout. Preferably
a view that covers the entire phone window.
`final View activityRootView = findViewById(R.id.activityRoot);` 
From that view you acquire it’s ViewTreeObserver. A ViewTreeObserver object can 
be retrieved from any view in your layout hierarchy using the getViewTreeObserver 
method. The ViewTreeObserver object has methods on it which allow you to register 
listeners for events fired when global changes happen in the view hierarchy.
There are several of these events you can tie into like onDraw or onPreDraw.
Very useful events to know about.

Specifically for this hack you want to tie into onGlobalLayout. You do this by 
calling addOnGlobalLayoutListener on the ViewTreeObserver.
You pass a new instance of OnGlobalLayoutListener to this method which you will 
use to detect the keyboard.

When the software keyboard is shown on screen it will cause a global layout pass, 
and this listener you just attached will fire. The number of times it fires seems 
a little unpredictable so you want to be prepared with defensive logic for this 
firing multiple times when the keyboard is shown.

`activityRootView.getViewTreeObserver().addOnGlobalLayoutListener(new OnGlobalLayoutListener() { @Override public void onGlobalLayout() { int heightDiff = activityRootView.getRootView().getHeight() - activityRootView.getHeight(); if (heightDiff > dpToPx(this, 200)) { // if more than 200 dp, it's probably a keyboard... // ... do something here } } });`

Inside the global layout listener you want to do a height check comparing your 
root layouts height to the height of the window. You can get the height of the
phone window by calling getRootView on the view that you attached the listener
to. This will return the top most view in the hierarchy which is most likely a 
DecorView, not the top view your layout. That DecorView will have the height of
your phone window.

So just to clarify that for you. You are getting the root of your layout, 
attaching a listener to it, and asking that root view for the top most 
phone window view. This is the view containing everything being drawn 
on the phone right now, beyond your current layout. That top most view,
is what you will use to get the phone window height.

The goal of the solution is to wait until the height difference change 
between your root layout view and the phones window are greater than a 
certain pixel amount. At the time the recommend was 128px, now it is 
about 200dp. Be sure to do a DP calculation.

It is important to note that this only works in adjustResize mode. If 
you are using adjustPan the height of your root layout view will not 
change when the keyboard comes up.

When that height difference happens you can “assume” and I don’t say assume 
lightly here. You can assume that the keyboard has just come up.

Side note; this reminds me of one of my favourite Android deprecations. 
I will never forget the method addGlobalOnLayoutListener being deprecated 
in favour of addOnGlobalLayoutListener. Funny little anecdote from the past.

So through much trial and tribulation I came up with a solution that I 
put in a utility class so that I could easily attach and detach keyboard
listeners throughout my code. And now I was able to hide those annoying 
UI items that I didn’t need while the keyboard was up.

I did this by taking the hack code putting it in a static method that 
takes an activity, and another interface callback. I called the callback
when the height check returned true for our keyboard up assumption. 
Important to note, I also needed another static method to remove the 
layout listener, since the listener contained a reference to an Activity.
Being so I had to have the addKeyboardListener method return the on 
global layout listener I just instantiated. This was so the instantiate 
listener could be passed to the remove method, so I had instance to use 
for removing the global layout listener. More nonsense for a silly little
hack, though making it modular allowed it to be used easily in other 
places in the code.

That didn’t solve everything though...
We started running into more UI issues. Positioning all those edit 
texts nicely in the mode ajustResize was difficult. And the UX guys
wanted the to be more predictable when the keyboard came up. So we 
returned to adjustPan because they decided they liked the positioning
better. Important note this what to used to, it being the default 
Android activity input method behaviour. A lot of apps behave in that
way. So it’s funny how you go down a rabbit hole for something, and 
then someone decides they liked it was. Shrug.

Also we had some PopupWindows that we attached to each edit text 
if it had an error. Those popups would move around as soon as 
keyboard came up andn lose their proper positioning. They were no 
longer located near the edittexts they were anchored to on creation.

So I still needed my keyboard listener, they wanted the UI elements like the logo to still hide, and I needed to know when to readjust the position of the Popup Windows. I was in adjustPan and the keyboard listener doesn’t work in that mode. So my solution became even hackier and the keyboard hack rather than relying on pixels was now a maze of booleans tracking layout calls. I can’t even remember how I made it reliable, but somehow I did and it wouldn’t be years later that I would get bit by being in adjustPan mode.

To solve the Popup Windows I just dismissed and re showed them when the keyboard came up. This helped keep them positioned correctly with the edit text they belonged to. 

Not sure how I would’ve done all that without the keyboard listener. But next time I’m up against similar problems I’ll need a different solution, because I won’t be using the ViewTreeObserver keyboard hack anymore.
Why? Well...

Fast forward 4 years, and I am working on a new app for another client. And here I am I need custom behaviour to happen when the software keyboard comes up. I’ve also all but forgotten about hardware keyboards at this point. Hell its 2017 who uses hardware keyboards or other alternative input methods. So I actually developed app dependent behaviour on the software keyboard being shown or not. Silly mistake.

For this app I was working on I needed a specific view to appear after the keyboard was dismissed. And when the keyboard came up I needed that view to also disappear. 

The view showing and hiding was the result for calculations. It would come up from the bottom of the screen above all the other content. This was how the app was designed in the UX docs I had, and this how it worked on iOS. Eek I was copying iOS app UI… also a bit of a mistake on my part.

If the result was up while the user focused an edit text, the results view would obstruct that edit text, because of adjustPan mode.

 So I broke out my old keyboard listener code, and reworked it so I could now properly detect the keyboard appearing and disappearing. Sadly I made a bigger mistake and because my old project was in adjustPan, I forgot that the keyboard hack required adjustResize to work properly.

So… I get it in a good place and it is working on every phone I am testing on, and we release the app into beta.

The user must dismiss the keyboard to see the results for calculation…. What can go wrong right?

Low and behold, and thankfully one beta user who had a new Pixel XL phone. The keyboard hack did not work on their phone.  And no matter how hard I tried, I could not get it to work. I even got it working on a pixel xl emulator, and not the device. I did not have access to a pixel, I do now woo hoo thankfully. Great phone. 

Many thanks to that beta user, on going back and forth a few times, before I got it working.

How? Well, I finally realised what I was using was a hack and not a reliable solution.

The ViewTreeObserver was firing more often than I was expecting, and numbers for the heights were not adding up as they should. The hack was beat, and I am glad that it was. I’m sure with enough time and effort I could’ve got it working on a pixel, but I chose a different solution. And it was not until I wrote and did research for this podcast that I remembered that I had forgotten to switch my activity to adjustResize, which was probably the source of truth behind things not working the way I was expecting.

Regardless this caused me to go back to the drawing board and actually look at the UX we had. I came up with a UX that was not the desired UX but was a UX that would work with the platform it was to run on, Android. I tried several things until, it popped into my head how dead simple this was to solve.

What I did is I took that result view and instead of sliding it up over top of the current content, well, I put it inline with the content and hid it by setting it’s visibility to GONE. 

If there were enough inputs filled out to make a calculation the results would become visible and the content would be pushed up, and the user could still see all the content, keyboard, and the edit text they were editing.

What I learned from all this, well if your code needs to make an assumption, maybe it’s not necessary to make those guesses. Sometimes it’s ok to look at the UX you are trying to implement and find a better more platform appropriate alternative. It is ok to say no to UX design, but have a suitable alternative to back it up and replace the solution you are refuting.

Assumptions aren’t all evil, a lot of subsystems that we rely on work in this way. But the less assumptions we make, the more solid or code will be.

And that is what this ViewTreeObserver solution is. It is just an assumption. Just because there is a height discrepancy between the DecorView and root view of your layout does not 100% guarantee you that the software keyboard is up. Making guesses like this when you are programming is dangerous, and they are prone as I’ve come to see to biting you with your own mistakes. Plus one day that assumption won’t hold true. For me it at the time it was 128px, now you have to be careful to make sure you do a DP calculation for different screen densities and make your assumption a 200dp height differential.

So please, listen to the wise words of Google and some of the better Android mentors out there, and reconsider not relying on the software keyboard view tree observer hack.

But for every coin my friends, there are two sides.

Alright! now let’s flip the tables and argue for having this functionality added to the Android platform. It is risky that a developer would attach important behaviour to such a callback, and then users with hardware keyboards would miss out on important functionality. For example what if someone is on an Android TV and uses a hardware keyboard, the calculator app I was working on would’ve appeared broken! Nothing would’ve happened, no calculations.

If this functionality was to be added it would probably be best that it was added in a way that supported hardware keyboards. Perhaps you could attach a listener for the software keyboard but also be notified by the system while attaching this listener that no software keyboard is available. If you were notified of this as a programmer, you developer could add proper logic for the edge case where there is no software keyboard.

Or perhaps you the developer would have to do your own due diligence and query the system to ask which keyboard is available before attaching the listener. If you didn’t do this query, a lint rule could notify the programmer that they forgot to check the keyboard type before attaching the listener.

I imagine it being something like this;

Acquire an input method service reference. Do a conditional check on that reference with the newly added api method I am suggesting. Lets call softwareKeyboardIsAvailable. If this conditionaly is true, then we use another new api method I am suggesting. The input service could have a method called attachSoftwareKeyboardListener. 

```
InputMethodManager input = context.getSystemService Context.INPUT_METHOD_SERVICE

If (input.isSoftwareKeyboard) input.addSoftwareKeyboardStateListener()
```

Though that probably is not as safe as perhaps this third idea. Instead the keyboard listener could be attached regardless of having a software keyboard or not. And it will always be called even in the case when the software keyboard would be shown but wasn’t. It could pass a boolean value indicating when the keyboardShow listener method is called whether the software was actually shown, or would’ve been shown.

So there we have three proposals for some new APIs to notify the developer of the software keyboard state changes. 

One, open up an api to allow a keyboard state listener be registered, during registration notify the developer if the listener is valid.

Two, also open up an api to allow a keyboard state listener, plus add an api call to query whether the software keyboard is available to be shown. Include lint rules to encourage the use of the querying method.

Three, have the listener report in its callback method whether the keyboard was shown, or would’ve been shown if it was available.

I honestly would like to see one of these three or a variation upon the same idea added to the Android system, it always good when we can stop people from going to Stackoverflow and using a popular hack. 

My argument for adding this to the Android platform as an API for us developers to use is people are doing it anyway, so let’s help the do it in a safer way that is more reliable than using a hack.

While that’s my 2 cents on the android software keyboard.

Bonus addition here, while I was just looking at the InputMethodManager while researching for this episode. I noticed a method that had slipped by me. InputMethodManager.isActive. I threw together a quick project to test if it works how I am assuming it works. Looking at it my first reaction was, oh this will tell me if the software keyboard is up. That will be better in a global layout listener than comparing pixel values.

I threw together a quick test project to see if this would work. Low and behold this method isActive doesn’t do what I was thinking. It appears to me it just means that a view that can accept input is currently active and focused… oh well…

```override fun onCreate(savedInstanceState: Bundle?) {
   super.onCreate(savedInstanceState)
   setContentView(R.layout.activity_main)

   var keyboardUp = false

   findViewById<View>(R.id.root).viewTreeObserver.addOnGlobalLayoutListener {
       val manager: InputMethodManager = getSystemService(Context.INPUT_METHOD_SERVICE) as InputMethodManager;
       if (manager.isActive && !keyboardUp) {
           keyboardUp = true
           AlertDialog.Builder(this).setMessage("Keyboard up").show()
       } else if (!manager.isActive && keyboardUp) {
           keyboardUp = false
           AlertDialog.Builder(this).setMessage("Keyboard down").show()
       }
   }
}
```



Anyhow, Maybe you know something about the keyboard I don’t! Hit me up if you do.

This has been a Segement. Thank you for joining me.

I’m Dave Anthony Thomas, and you can reach me @dave_a_thomas on twitter. 

If you’d like to comment on this podcast please join me on github at davethomas11/segments and join the discussion in the github issues.

 see you next time, on Segments!

Cheers.
