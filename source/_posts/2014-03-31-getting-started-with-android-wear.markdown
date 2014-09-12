---
layout: post
title: "Getting Started with Android Wear"
date: 2014-03-31 19:05:25 +0100
comments: true
categories: 
- Android
- Wearables
teaserimage: /images/2014-03-31-getting-started-with-android-wear/androidwear-2.png
asins:
- "1933820985" # Make It So: Interaction Design Lessons from Science Fiction (Natham Shedroff, Christopher Noessel)

---

Wearables are one of the hot topics of the year. No - I am not talking about T-shirts with embroidered [flashing charts](http://blog.divineforge.com/2010/09/flashwear-birt-rocks-t-shirt.html). Rather, let’s consider gadgets like [Pebble](https://getpebble.com/), [Oculus Rift](http://www.oculusvr.com/), and [Google Glass](http://www.google.com/glass/start/). Besides being geeky pieces of technology (and who of us wouldn’t enjoy playing around with gadgets), they mark the beginning of a whole new era of computing. 

<!-- more -->

Until now, using a computer usually meant interacting with a conglomeration of metal, silicon and other materials that could more or less clearly be perceived as a computer. Even smartphones - the latest step in the evolution of computers - are rather clumsy to use, if you think about it.

Perhaps the most striking way to see the difference between regular computers and wearables is the user’s posture when using the computer. Using a computer results in a more or less non-natural posture or - even worse - behaviour (see [Curious Rituals](http://curiousrituals.wordpress.com/2012/09/03/curious-rituals-book/) for a look in the mirror). 

Wearables change this. By being attached to or located closely to your body, they allow you to take up a much more natural posture and eventually (in the more or less distant future), people around you will no longer notice them.

Granted, in their current incarnation, many wearables are still a long way from this vision and it is remains to the readers imagination how long you can wear a VR device like the Oculus Rift without getting a sore neck or becoming nauseous. However, this is only just the start and we will see great progress in the following years.

On March 18th, Google announced [Android Wear](http://googleblog.blogspot.co.uk/2014/03/sharing-whats-up-our-sleeve-android.html) and released the [Android Wear Developer Preview](http://android-developers.blogspot.co.uk/2014/03/android-wear-developer-preview.html). Shortly after that, Motorola announced their [Moto 360 watch](http://motorola-blog.blogspot.co.uk/2014/03/moto-360-its-time.html) which generated quite some excitement on the internet. On the same day, LG announced their [LG G watch](http://www.lg.com/global/gwatch) - a rectangular model.

[Android Wear](http://www.android.com/wear/) is Google's second platform for wearable devices ([Google Glass](http://www.google.co.uk/glass/start/) being the first one). The theme for both platforms is to bring useful, contextual information to users when they need it. Ultimately, the goal is to empower users without distracting them.

If you’re excited about Android Wear as well, here are some ideas for you to try out.

## First of all - watch the videos

- [Introducing Android Wear Developer Preview](https://www.youtube.com/watch?v=0xQ3y902DEQ#t=17) with [+DavidSingleton](https://plus.google.com/+DavidSingleton/posts)
- [Android Wear Developer Preview](https://www.youtube.com/watch?v=1dQf0sANoDw&list=PLWz5rJ2EKKc-kIrPiq098QH9dOle-fLef) with [+JustinKoh](https://plus.google.com/+JustinKoh)
- [Android Wear: receiving Voice Replies](https://www.youtube.com/watch?v=SEZbZK4jFLY) with [+JustinKoh](https://plus.google.com/+JustinKoh)

## If you're a developer

1. Sign up for the Developer Preview. Head over to [our developer site](http://developer.android.com/wear/preview/start.html) and sign up for the Developer Preview. When signing up, **please make sure to use the same Gmail account you use on your Android device** - this will make it much easier for you to download the Android Wear Preview beta app to your phone or tablet. As soon as you’ve been accepted to the preview, you will receive a confirmation email that outlines how to opt-in to the Android Wear Preview app beta program. You’ll need this app in order to connect your Android Wear simulator to your device. 

2. Follow the instructions for setting up the Developer Preview on your developer machine as described in [Getting Started with the Developer Preview](http://developer.android.com/wear/preview/start.html). Here are a few things to make sure it all works out fine:
	- use the same Gmail account for signing up that is connected to your test device
	- make sure to use a device running Android 4.3 or better, as this is a prerequisite for the Preview app
	- after installing and starting the Android Wear Preview on your Android device, make sure to tap the blue note on the preview app to register the app as a notification listener
	
The great thing about Android Wear is that your app's notifications will show up on any connected Android Wear device (or emulator, for that matter) [without any further effort](http://developer.android.com/wear/notifications/creating.html). So, if you have an app that uses notifications to alert the user, you should be able to see those notifications on the Android Wear Emulator as soon as you have connected the Emulator to your Android device (phone or tablet) running your app. If you don't have an app that uses notifications, you can quickly check this out by sending an email to the Gmail account cyou use on your device so the notification for the email pops up in the notification center.

Notifications are one of the [two major design principles](http://developer.android.com/wear/design/user-interface.html) of Android Wear: **Suggest** and **Demand**.

### Suggest: The Context Stream

<div class="wear-inset-video-container" style="float:left;margin:0 0px 20px -30px">
  <img class="wear-bezel-only" src="/images/2014-03-31-getting-started-with-android-wear/bezel.png" alt="">
  <img class="gif" src="/images/2014-03-31-getting-started-with-android-wear/stream.gif">
</div>

All notifications will end up on the context stream, which is a vertical list of cards, much like Google Now on Android phones. The user can interact with the cards by swiping them vertically, thereby navigating the stream of important information. Due to the limited screen real estate, only one card will be displayed at a time. You can add cards to the context stream by creating and delivering notifications. Use [NotificationCompat.Builder](https://developer.android.com/reference/android/preview/support/v4/app/NotificationManagerCompat.html) to create basic notifications (make sure to use the one in the Android Wear Preview SDK, otherwise some features will not work). To use Android Wear specific features like pages or actions, or to customize the look of cards on the context stream, use [WearableNotifications.Builder](http://developer.android.com/reference/android/preview/support/wearable/notifications/WearableNotifications.Builder.html). Here is a short snippet that shows how to create a notification:


```java
int notificationId = 001;

// Build intent for notification content
Intent viewIntent = new Intent(this, ViewEventActivity.class);
viewIntent.putExtra(EXTRA_EVENT_ID, eventId);
PendingIntent viewPendingIntent =
        PendingIntent.getActivity(this, 0, viewIntent, 0);

NotificationCompat.Builder notificationBuilder =
        new NotificationCompat.Builder(this)
        .setSmallIcon(R.drawable.ic_event)
        .setContentTitle(eventTitle)
        .setContentText(eventLocation)
        .setContentIntent(viewPendingIntent);

// Get an instance of the NotificationManager service
NotificationManagerCompat notificationManager =
        NotificationManagerCompat.from(this);

// Build the notification and issues it with notification manager.
notificationManager.notify(notificationId, notificationBuilder.build());
```

Whenever you create notifications, be sure to follow the [Android Wear Design Principles](http://developer.android.com/wear/design/index.html. In particular, you should make sure all information you deliver follows the principle of being _timely_, _relevant_, and _specific_.

### Demand: The Cue Card

<div class="wear-inset-video-container" style="float:right;margin:0 -30px 20px 0px">
  <img class="wear-bezel-only" src="/images/2014-03-31-getting-started-with-android-wear/bezel.png" alt="">
  <img class="gif" src="/images/2014-03-31-getting-started-with-android-wear/cuecard.gif">
</div>

Android Wear is not just about displaying notifications that the user can then interact with. In addition, users can initiate an interaction with their device by using the cue card. The cue card is brought up by saying "OK Google" or tapping on the "g" icon on the home stream of the device. Swiping up on the cue card will reveal a number of actions that can be executed by tapping them or speaking them. Voice actions are not yet available in the current preview SDK.

### A word of caution

As the name implies, the Android Wear Developer Preview is not intended for creating production apps at this time. The APIs might change significantly prior to the official release of Android Wear. The preview is intended for development and testing purposes only. Also, we're interested in your feedback, so make use of the [Google+ community](https://plus.google.com/communities/113381227473021565406) and [Stack Overflow](http://stackoverflow.com/questions/tagged/android-wear).


## If you're a designer

(or a developer who wants to understand the design ideas behind Android Wear)

1. Read the [Android Wear Design Principles](http://developer.android.com/wear/design/index.html).

2. Become a member of the [Android Wear Google+ Community](https://plus.google.com/communities/113381227473021565406) to get in touch with fellow developers / designers and share your ideas.

3. A few people have created some great design resources for Android Wear. If you’re interested in creating some mock-ups, check out [+TaylorLing](https://plus.google.com/+TaylorLing)’s [Android Wear UI Design Kit for Photoshop](http://androiduiux.com/2014/03/19/android-wear-ui-design-kit-for-photoshop-0-1-free-download/). If you want to add some more glitz, head over to [+SpiderflyStudios](https://plus.google.com/+SpiderflyStudios) and download their Moto 360 mockups. Photoshop is not your thing? Check out [Frank Erlich](http://frankerlich.com/)’s [OmniGraffle stencil](https://www.graffletopia.com/stencils/1257).

{% fancyalbum 200x150 %}
/images/2014-03-31-getting-started-with-android-wear/android_wear_design_kit.png: Android Wear UI Design Kit for Photoshop
/images/2014-03-31-getting-started-with-android-wear/spiderfly_moto360_mockup.png: Spiderfly Moto 360 Mockup
/images/2014-03-31-getting-started-with-android-wear/omnigraffle_stencil.png: Frank Erlich's Omnigraffle Stencil
{% endfancyalbum %}

**Please note** that the current SDK focusses entirely on notifications. Refer to the Design Principles of Android Wear for a good overview of the [Notifiaction UI Patterns available](http://developer.android.com/wear/design/index.html). Let this be your guide when using the design resources I linked. Not everything that's possible in Photoshop might be possible with the current SDK.

## Lastly...

...here is the link to the music used in the video: [Chromeo – Jealous (I Ain't With It)](http://open.spotify.com/track/7aC7xWyT2LJ4R5r18GymA2)

<iframe src="https://embed.spotify.com/?uri=spotify:track:7aC7xWyT2LJ4R5r18GymA2" width="300" height="380" frameborder="0" allowtransparency="true"></iframe>

Enjoy!