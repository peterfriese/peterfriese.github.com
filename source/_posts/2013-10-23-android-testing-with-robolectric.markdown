---
layout: post
title: "Android Testing with Robolectric"
date: 2013-10-23 20:02
comments: true
categories: 
- Android
- Testing
- Robolectric
asins:
- "1118102274" # Professional Android 4 Application Development (Reto Meier)
- "1937785017" # Cucumber Recipes: Automate Anything with BDD Tools and Techniques (Ian Dees, Matt Wynne, Aslak Hellesoy)
teaserimage: /images/2013-10-23-android-testing-with-robolectric/robolectric.png

---

We all know testing is essential to verify the behaviour of the software you write. In the Android world with its huge variety of devices and OS versions, this is even more so. Unfortunately, the performance of the Android development environment, most notably the Android Emulator, leaves much to be desired which sometimes leads to neglicence of test discipline. Robolectric is a test framework that tries to alleviate this situation.

<!-- more -->

Robolectric achieves this by sitting between your code and the Android OS code intercepting calls and redirecting them to shadow objects, making it possible to run your tests inside a regular JVM. This effectively means you can run your tests on your desktop - no waiting for your code to be deployed to the Emulator or a physical device!

For more detailed background information about the inner workings of Roboloectric, check out [this presentation](http://www.slideshare.net/tylerschultz/robolectric-android-unit-testing-framework) and this [talk by Tyler Schultz](http://www.youtube.com/watch?v=T6FWL877txw), one of the committers of Robolectric. 

## Creating a New Project

To better understand how Robolectric works, let's create a very simple project we'll use as our object under test. We'll use Android Studio (at the time of writing, version 0.3.0 was the latest version).

Create a new project using the settings you can see in the following screenshots. Choose __Blank Activity__ and __Navigation Type None__.

{% fancyalbum 200x150 %}
/images/2013-10-23-android-testing-with-robolectric/new_project_1.png: New Create New Project - Step 1
/images/2013-10-23-android-testing-with-robolectric/new_project_2.png: New Create New Project - Step 2
/images/2013-10-23-android-testing-with-robolectric/new_project_3.png: New Create New Project - Step 3
{% endfancyalbum %}


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

{% fancyimage center /images/2013-10-23-android-testing-with-robolectric/robolectric_app_ui.png 200x400 Application Under Test UI%}

## Preparing for Testing

To prepare your project for testing with Robolectric, there are a few things you need to do:

1. Create a folder for your tests and make sure both Gradle and Android Studio recognize it.
2. Add the Gradle Android Test Plug-in to your project
3. Add a customized TestRunner


### Creating a Folder For Your Tests

When you create a new project in Android Studio, it is not yet configured for testing. If you used to develop Android apps in Eclipse, you probably are used to creating a new project for your tests. The good news is, with the new Android build system based on Gradle, you can now place your tests in the same project as your production code (refer to the [build system documentation](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Testing) for more details).

However, Android Studio (and the Gradle Plug-in) currently expect you to place tests in `src/instrumentTest`. As we won't be writing instrument tests, we'd rather like to place our tests in `src/test`. In order for this to work, we need to tell Gradle and Android Studio we're deviating from the default project structure:

1. Create a new folder `src/test/java` to contain your tests.
2. Edit `build.gradle` in your project folder and add the following source set configuration to the Android plug-in configuration:

``` groovy
		sourceSets {
			instrumentTest.setRoot('src/test')
		}
```

3. Next, in Android Studio, select `Tools -> Android -> Sync Project with Gradle Files` from the main menu. This will tell Android Studio where to find your test files.

You should notice how the icon for your test files folder turns from orange to green:

{% fancyimage center /images/2013-10-23-android-testing-with-robolectric/robolectric_test_folder.png 200x150 Test Folder Setup%}

### Adding the Gradle Test Plug-in

In order to be able to run Android unit tests with Gradle, we need to add the [Gradle Android Test plug-in](https://github.com/JakeWharton/gradle-android-test-plugin) to the build script.

1. Add the maven snapshots repository to the build script repositories and regular repositories section. This is in order to get the fix provided in [pull request 26](https://github.com/JakeWharton/gradle-android-test-plugin/pull/26).
```groovy
		maven {
			url 'https://oss.sonatype.org/content/repositories/snapshots/'
		}
```		
2. Add the plug-in to the dependencies section of your `build.gradle`file:

		classpath 'com.squareup.gradle:gradle-android-test-plugin:0.9.1-SNAPSHOT'
		
3. Apply the plug-in:
```groovy
		apply plugin: 'android-test'
```		
4. Add test-only dependencies:
```groovy
		testCompile 'junit:junit:4.10'
		testCompile 'org.robolectric:robolectric:2.1.+'
		testCompile 'com.squareup:fest-android:1.0.+'
		instrumentTestCompile 'junit:junit:4.10'
		instrumentTestCompile 'org.robolectric:robolectric:2.3-SNAPSHOT'
		instrumentTestCompile 'com.squareup:fest-android:1.0.+'
```
		
Your `build.gradle` should now look like this:
```groovy
	buildscript {
		repositories {
			mavenCentral()
			maven {
				url 'https://oss.sonatype.org/content/repositories/snapshots/'
			}
		}
		dependencies {
			classpath 'com.android.tools.build:gradle:0.6.+'
			classpath 'com.squareup.gradle:gradle-android-test-plugin:0.9.1-SNAPSHOT'
		}
	}
	apply plugin: 'android'
	apply plugin: 'android-test'
	
	repositories {
		mavenCentral()
		maven {
			url 'https://oss.sonatype.org/content/repositories/snapshots/'
		}
	}
	
	android {
		compileSdkVersion 18
		buildToolsVersion "18.1.0"
	
		defaultConfig {
			minSdkVersion 7
			targetSdkVersion 18
		}
	
		sourceSets {
			instrumentTest.setRoot('src/test')
		}
	}
	
	dependencies {
		compile 'com.android.support:appcompat-v7:+'
	
		testCompile 'junit:junit:4.10'
		testCompile 'org.robolectric:robolectric:2.3-SNAPSHOT'
		testCompile 'com.squareup:fest-android:1.0.+'
		instrumentTestCompile 'junit:junit:4.10'
		instrumentTestCompile 'org.robolectric:robolectric:2.3-SNAPSHOT'
		instrumentTestCompile 'com.squareup:fest-android:1.0.+'
	
	}
```

### Adding a Customized Test Runner

Because of the way Robolectric looks for your `AndroidManifest.xml` during test startup, it won't find the manifest if your tests are located in `src/test/java`. To alleviate this situation, we need to add a custom test runner.

Add the following file to your project, making sure to place it somewhere in the `test` tree:

``` java RobolectricGradleTestRunner.java
	import android.app.Fragment;
	import android.app.FragmentManager;
	import android.app.FragmentTransaction;
	import android.support.v4.app.FragmentActivity;
	
	import org.junit.runners.model.InitializationError;
	import org.robolectric.AndroidManifest;
	import org.robolectric.Robolectric;
	import org.robolectric.RobolectricTestRunner;
	import org.robolectric.annotation.Config;
	import org.robolectric.res.Fs;
	
	public class RobolectricGradleTestRunner extends RobolectricTestRunner {
	
		public RobolectricGradleTestRunner(Class<?> testClass) throws InitializationError {
			super(testClass);
		}
	
		@Override
		protected AndroidManifest getAppManifest(Config config) {
			String manifestProperty = System.getProperty("android.manifest");
			if (config.manifest().equals(Config.DEFAULT) && manifestProperty != null) {
				String resProperty = System.getProperty("android.resources");
				String assetsProperty = System.getProperty("android.assets");
				return new AndroidManifest(Fs.fileFromPath(manifestProperty), Fs.fileFromPath(resProperty),
						Fs.fileFromPath(assetsProperty));
			}
			AndroidManifest appManifest = super.getAppManifest(config);
			return appManifest;
		}
	
	}
```

## Writing Your First Robolectric Test

Let's write our first test! Add the following class to `src/test/java`:

``` java MainActivityTest.java
	package de.peterfriese.robolectricdemo;
	
	import org.junit.Test;
	import org.junit.runner.RunWith;
	
	import de.peterfriese.robolectric.RobolectricGradleTestRunner;
	
	import static org.junit.Assert.assertTrue;
	
	@RunWith(RobolectricGradleTestRunner.class)
	public class MainActivityTest {
	
		@Test
		public void shouldFail() {
			assertTrue(false);
		}
	}
```

A couple of things to note here: 

Firstly, we use the newly created `RobolectricGradleTestRunner`to make sure the test can access the Android manifest (and thereby the other Android components in your app).

Secondly, we added a simple test method, making suer it will fail. Why is this a good idea? Well, it makes sure our test fails when running it, so we can be sure the whole setup actually works. If we run the test and it *does not* fail, we know that something is wrong.

So, let's run the test!
``` sh
	$ ./gradlew test
	
	de.peterfriese.robolectricdemo.MainActivityTest > shouldFail FAILED
	java.lang.AssertionError at MainActivityTest.java:15

	1 test completed, 1 failed
	:RobolectricDemo:testDebug FAILED

	FAILURE: Build failed with an exception.

	* What went wrong:
	Execution failed for task ':RobolectricDemo:testDebug'.
	> There were failing tests. See the report at: file:///Users/peterfriese/Projects/peterfriese.de/Robolectric/RobolectricDemoProject/RobolectricDemo/build/test-report/debug/index.html

	* Try:
	Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.

	BUILD FAILED
```

So, the test failed - just as we expected. You can open the test report (`../RobolectricDemoProject/RobolectricDemo/build/test-report/debug/index.html`) in your browser to get more detailed information about the test run.


## Enhancing the Test

Now that we've got everything in place, let's develop a small feature in a test-driven way. You already noted how we enhanced the UI of the application at the beginning, but apart from looking nice, there's not much functionality.

Let's enhance the app so the user can enter their name, press a button and be greeted by the application.

Make sure to statically import the Android FEST assertations - they'll help us to write our tests in a fluent manner:

``` java
	import static org.fest.assertions.api.ANDROID.assertThat;
```

As we need to have access to the app's main activity in all of our tests, let's add a private field for it and create a setup method that will create a fresh activity before each test is run:

``` java
	private MainActivity activity;

	@Before
	public void setup() {
		activity = Robolectric.buildActivity(MainActivity.class).get();
	}
```

It is a good practice to ensure the UI is fired up and all UI elements have been initialized as expected (i.e. are not null):

``` java
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

```java
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

``` java MainActivity.java
	public static class MainFragment extends Fragment {
		@Override
		public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
			final View rootView = inflater.inflate(R.layout.fragment_main, container, false);
			Button button = (Button) rootView.findViewById(R.id.button);
			button.setOnClickListener(new View.OnClickListener() {
				@Override
				public void onClick(View view) {
					TextView textView = (TextView) rootView.findViewById(R.id.textView);
					EditText editText = (EditText) rootView.findViewById(R.id.editText);

					textView.setText(String.format("Hello, %s!", editText.getText()));
				}
			});

			return rootView;
		}
	}
```
	
As you can see, this code will register an `OnClickListener` for the button and set the text of the text view according to the value of the edit field.
	
Also, change the constructor of `MainActivity` so it uses the new fragment class when instantiating the view fragment:

``` java
		if (savedInstanceState == null) {
			getSupportFragmentManager().beginTransaction()
					.add(R.id.container, new MainFragment())
					.commit();
		}
```

The test should now pass:

``` sh
	$ ./gradlew test
	BUILD SUCCESSFUL

	Total time: 7.066 secs	
```

## Conclusion

Setting up Robolectric so that you can use both Gradle and Android Studio may not be as simple as it should be, but it's reasonably easy with the information I have given you above. Adding Robolectric to a new Android project is just a matter of minutes with these instructions and helps you save a lot of time you'd have had to spend otherwise running the tests on the emulator or on physical devices.

The source code for this blog post is [available on Github](https://github.com/peterfriese/RobolectricDemoProject).