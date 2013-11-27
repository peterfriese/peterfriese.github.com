---
layout: post
title: "Unit Testing Android Apps with Robolectric and Eclipse"
date: 2013-11-27 09:54
comments: true
categories: 
- Android
- Testing
- Robolectric
- Eclipse
- ADT
- Android Emulator
asins:
- "3943747042" # Eclipse IDE: Eclipse IDE based on Eclipse 4.2 and 4.3 (Lars Vogel)
- "1937785017" # Cucumber Recipes: Automate Anything with BDD Tools and Techniques (Ian Dees, Matt Wynne, Aslak Hellesoy)
teaserimage: /images/2013-10-23-android-testing-with-robolectric/robolectric.png

---

A few weeks ago, I showed you how to set up Robolectric with Android Studio and Gradle. For the Eclipse DemoCamp in Hamburg, I have been asked to prepare a session on how to use Robolectric with Eclipse. I thought it might be worthwhile to share my experiences with everybody who wasn't fortunate enough to attend the DemoCamp (in a truely stunning location, BTW), so here goes.

<!-- more -->

We all know testing is essential to verify the behaviour of the software you write. In the Android world with its huge variety of devices and OS versions, this is even more so. Unfortunately, the performance of the Android development environment, most notably the Android Emulator, leaves much to be desired which sometimes leads to neglicence of test discipline. Robolectric is a test framework that tries to alleviate this situation.

Robolectric achieves this by sitting between your code and the Android OS code intercepting calls and redirecting them to shadow objects, making it possible to run your tests inside a regular JVM. This effectively means you can run your tests on your desktop - no waiting for your code to be deployed to the Emulator or a physical device!

For more detailed background information about the inner workings of Roboloectric, check out [this presentation](http://www.slideshare.net/tylerschultz/robolectric-android-unit-testing-framework) and this [talk by Tyler Schultz](http://www.youtube.com/watch?v=T6FWL877txw), one of the committers of Robolectric.

## Creating a New Project

As the blog title implies, this time around, we'll use Eclipse to create our project under test. Make sure you're using the most recent version of the Eclipse ADT, which is part of the Android SDK. If you don't have it already, Google provides a nice all-in-one download of the SDK [here](http://developer.android.com/sdk/index.html).

Create a new project, using the following settings:

* Application Name: `RobolectricDemoProjectEclipse`
* Project Name: `RobolectricDemoProjectEclipse`
* Package Name: `de.peterfriese.robolectricdemo`
* Minimum SDK: `API 8: Android 2.2 (Froyo)`
* Target SDK: `API 18: Android 4.3`
* Compile With: `API 19: Android 4.4 (KitKat)`

Choose to create a custom icon and a custom launcher and place the project in a location of your liking.

{% fancyalbum 200x150 %}
/images/2013-11-27-unit-testing-android-apps-with-robolectric-and-eclipse/robolectric_new_project_1.png: New Create New Project - Step 1
/images/2013-11-27-unit-testing-android-apps-with-robolectric-and-eclipse/robolectric_new_project_2.png: New Create New Project - Step 2
/images/2013-11-27-unit-testing-android-apps-with-robolectric-and-eclipse/robolectric_new_project_3.png: New Create New Project - Step 3
{% endfancyalbum %}

On the *Create Activity* page, choose *Blank Activity*, and on the *Blank Activity* configuration page use the following settings:

* Activiy Name: `Main Actity`
* Layout Name: `activity_main`
* Navigation Type: `None`

To make things a bit more interesting, let's change the UI so we've got a `TextView`, an `InputView` and a `Button` on, just like this:

```xml res/layout/fragment_main.xml
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    xmlns:tools="http://schemas.android.com/tools"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:paddingLeft="@dimen/activity_horizontal_margin"
	    android:paddingRight="@dimen/activity_horizontal_margin"
	    android:paddingTop="@dimen/activity_vertical_margin"
	    android:paddingBottom="@dimen/activity_vertical_margin"
	    tools:context=".MainActivity$PlaceholderFragment">
	
	    <TextView
	        android:text="@string/hello_world"
	        android:layout_width="wrap_content"
	        android:layout_height="wrap_content"
	        android:textAppearance="?android:attr/textAppearanceLarge"
	        android:id="@+id/textView"
	        android:layout_alignRight="@+id/editText"
	        android:layout_alignParentLeft="true"/>
	
	    <EditText
	        android:layout_width="wrap_content"
	        android:layout_height="wrap_content"
	        android:id="@+id/editText"
	        android:layout_below="@+id/textView"
	        android:layout_marginTop="24dp"
	        android:layout_alignParentRight="true"
	        android:layout_alignParentLeft="true"/>
	
	    <Button
	        android:layout_width="wrap_content"
	        android:layout_height="wrap_content"
	        android:text="Say Hi!"
	        android:id="@+id/button"
	        android:layout_below="@+id/editText"
	        android:layout_centerHorizontal="true"
	        android:layout_marginTop="45dp"/>
	
	</RelativeLayout>
```

{% fancyimage center /images/2013-11-27-unit-testing-android-apps-with-robolectric-and-eclipse/ui_layout.png 200x400 Application Under Test UI %}

## Preparing for Testing

To prepare your project for testing with Robolectric in Eclipse, there are a few things you need to do:

1. Create a test project
2. Add Robolectric and its dependencies
3. Help Robolectric to find the Android manifest

### Creating a Test Project

The great thing about Robolectric is your tests are plain JUnit tests, so there is no need to run the tests on a device or the Emulator. This means we can use a plain Java project to host our tests.

1. Create a new Java project, naming it `RobolectricDemoProjectEclipseTests`
2. On the second page of the wizard, add a project reference to the Android project (`RobolectricDemoProjectEclipse`)

{% fancyalbum 200x150 %}
/images/2013-11-27-unit-testing-android-apps-with-robolectric-and-eclipse/testproject_1.png: New Create New Test Project - Step 1
/images/2013-11-27-unit-testing-android-apps-with-robolectric-and-eclipse/testproject_2.png: New Create New Test Project - Step 2
{% endfancyalbum %}

### Adding Robolectric and its Dependencies

By now, you might be wondering why we don't use Maven to set up our dependencies. To cut a long story short: setting up an Android project in Eclipse is a lot easier without using Maven. The downside of this approach, however, is that we have to manage project dependencies ourselves.

Head over to [Maven Central](http://search.maven.org/#search%7Cga%7C1%7Cg%3A%22org.robolectric%22) to fetch the [latest version of Robolectric including dependencies](http://search.maven.org/remotecontent?filepath=org/robolectric/robolectric/2.2/robolectric-2.2-jar-with-dependencies.jar) (at the time of this writing, Robolectric 2.2 was the most up-to-date version).

In `RobolectricDemoProjectEclipseTests`, create a new folder `libs` and add the downloaded jar file by dragging it to the folder. After that, right-click the added file in Eclipse and choose _Build Path -> Add to Build Path_ to add it to the build path.

Later, we will use [FEST Android](http://square.github.io/fest-android/) to write our test assertations in a fluent way, so we need the required jar files and add the to the project as well.

* Latest [FEST Android jar](http://repository.sonatype.org/service/local/artifact/maven/redirect?r=central-proxy&g=com.squareup&a=fest-android&v=LATEST)
* Latest [FEST jar](http://search.maven.org/remotecontent?filepath=org/easytesting/fest-assert-core/2.0M10/fest-assert-core-2.0M10.jar)

As JUnit is the base for all our unit tests, we need to add it to our classpath, too. Open the project's properties, navigate to _Java Build Path -> Libraries_ and click on _Add Library..._. In the next dialog, choose _JUnit_ and press _Next_. Finaly, choose _JUnit 4_ (Robolectric relies on JUnit 4) and press _Finish_.

{% fancyalbum 200x150 %}
/images/2013-11-27-unit-testing-android-apps-with-robolectric-and-eclipse/add_junit_1.png: Adding JUnit 4 - Step 1
/images/2013-11-27-unit-testing-android-apps-with-robolectric-and-eclipse/add_junit_2.png: Adding JUnit 4 - Step 2
/images/2013-11-27-unit-testing-android-apps-with-robolectric-and-eclipse/add_junit_3.png: Adding JUnit 4 - Step 3
{% endfancyalbum %}


Finally, we need to add the Android library itself as a dependency to our test project. To do this, open then project properties dialog (Right-click the test project, select _Properties_) and navigate to _Java Build Path_. In the dialog, click on _Add External JARs..._ and navigate to `<Android SDK>/platforms/android-19/android.jar` to add the latest Android jar to the projects build path.

{% fancyalbum 200x150 %}
/images/2013-11-27-unit-testing-android-apps-with-robolectric-and-eclipse/add_android.png: Adding the Android jar
{% endfancyalbum %}
	
### Helping Robolectric to Find Your Manifest

Because of the way Robolectric looks for your `AndroidManifest.xml` during test startup, it won't find the manifest if your tests and the manifest are located in different projects.

Robolectric supports configuring individual tests using the `@Config` annotation, allowing you to use a different manifest file depending on the test you run. This can be quite handy, but it is a good idea to have a default configuration. Starting with version 2.0, Robolectric supports a global configuration using a config file.

To specifiy a manifest file, place a file named `org.robolectric.Config.properties` in the root of the `src` tree of your test project, so it can be discovered by Robolectric during test setup and add the following line:

```properties src/org.robolectric.Config.properties
	manifest=../RobolectricDemoProjectEclipse/AndroidManifest.xml
```
	
This will tell Robolectric to use the `AndroidManifest.xml` file from our Android project under test.


## Writing Your First Robolectric Test

Let's write out first test! Add the following class to `src/test/java`:

```java MainActivityTest.java
	package de.peterfriese.robolectricdemo;
	
	import org.junit.Test;
	import org.junit.runner.RunWith;
	import org.robolectric.Robolectric;
	import org.robolectric.RobolectricTestRunner;

	
	import static org.junit.Assert.assertTrue;
	
	@RunWith(RobolectricTestRunner.class)
	public class MainActivityTest {
	
		@Test
		public void shouldFail() {
			assertTrue(false);
		}
	}
```
	
Please note that we added a simple test method, making sure it will fail. Why is this a good idea? Well, it makes sure our test fails when running it, so we can be sure the whole setup actually works. If we run the test and it *does not* fail, we know that something is wrong.

So, let's run the test! Right-click on the test file and choose _Run As -> JUnit Test_. As a result, you should see the test bar turn red in the JUnit view.

The test failed - just as expected. You can turn it into a passing test by changing the assert statement to `assertTrue(true);`


## Enhancing the Test

Now that we've got everything in place, let's develop a small feature in a test-driven way. You already noted how we enhanced the UI of the application at the beginning, but apart from looking nice, there's not much functionality.

Let's enhance the app so the user can enter their name, press a button and be greeted by the application.

Make sure to statically import the Android FEST assertations - they'll help us to write our tests in a fluent manner:

``` java
	import static org.fest.assertions.api.ANDROID.assertThat;
```
	
As we need to have access to the app's main activity in all of our tests, let's add a private field for it and create a setup method that will create a fresh activity before each test is run:

```java MainActivity.java
	private MainActivity activity;

	@Before
	public void setup() {
		activity = Robolectric.buildActivity(MainActivity.class).get();
	}
```

It is a good practice to ensure the UI is fired up and all UI elements have been initialized as expected (i.e. are not null):

```java MainActivityTest.java
	@Test
	public void shouldNotBeNull() {
		assertThat(activity).isNotNull();

		TextView textView = (TextView) activity.findViewById(R.id.textView);
		assertThat(textView).isNotNull();

		Button button = (Button) activity.findViewById(R.id.button);
		assertThat(button).isNotNull();

		EditText editText = (EditText) activity.findViewById(R.id.editText);
		assertThat(editText).isNotNull();
	}
```

Next, let's write a test that places a text inside the edit field, presses the button and compares the result in the text view with the expected result.

Here is the test method:

```java MainActivityTest.java
	@Test
	public void shouldProduceGreetingWhenButtonPressed() {
		TextView textView = (TextView) activity.findViewById(R.id.textView);
		Button button = (Button) activity.findViewById(R.id.button);
		EditText editText = (EditText) activity.findViewById(R.id.editText);

		editText.setText("Peter");
		button.performClick();

		assertThat(textView).containsText("Hello, Peter!");
	}
```

When you run the test, it will obviously fail, so let's fix this.

Open `MainActivity.java` and insert the following private class:

```java MainActivity.java
	public static class MainFragment extends Fragment {
		@Override
		protected void onCreate(Bundle savedInstanceState) {
			super.onCreate(savedInstanceState);
			setContentView(R.layout.activity_main);
		
			Button button = (Button) this.findViewById(R.id.button);
			button.setOnClickListener(new View.OnClickListener() {
				@Override
				public void onClick(View view) {
					TextView textView = (TextView) MainActivity.this.findViewById(R.id.textView);
					EditText editText = (EditText) MainActivity.this.findViewById(R.id.editText);

					textView.setText(String.format("Hello, %s!", editText.getText()));
				}
			});
		}
	}
```
	
As you can see, this code will register an `OnClickListener` for the button and set the text of the text view according to the value of the edit field.
	
The test should now pass - just run the unit test again and you should see a green bar.

## Conclusion

Setting up an Android project for using Robolectric in Eclipse is quite a bit of work, as you just have seen, but it's reasonably easy with the information I have given you above. Adding Robolectric to a new Android project is just a matter of minutes with these instructions and helps you save a lot of time you'd have had to spend otherwise running the tests on the emulator or on physical devices.

You might be interested in checking out my recent post on integrating Robolectric in a Gradle-based Android project in Android Studio - [this way, please](/android-testing-with-robolectric/).

## Slides

Here are the slides I used for presenting this topic at [Eclipse Democamp November 2013 in Hamburg](http://wiki.eclipse.org/Eclipse_DemoCamps_November_2013/Hamburg):

{% slideshare 28679137 %}