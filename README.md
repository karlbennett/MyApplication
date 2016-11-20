MyApplication
=============

This project is a Maven translation of the 
[Build Your First App](https://developer.android.com/training/basics/firstapp/creating-project.html) android example 
application. It is here to show how you can create an equivalent project using Maven instead of Gradle. 

The reason you may want to do this is so that you can keep your Android projects in line with the rest of your Java 
projects. Also this Maven project is of course by it's nature much simpler than the Gradle project and will also build 
faster, have smoother IDE integration, provide a much more conventional/predictable structure, and is less prone to the 
build script degradation of a Gradle project.

### Prerequisites

Before this project will build you will need to carry out the following tasks. Though these tasks are just one offs, 
there is no need to repeat them between subsequent builds.

##### Install the Android SDK

To get the project to build you will need to do download and install the Android commandline tools. Of course if you 
wish to actually carry out Android development it would be a good idea to install the Android Studio, which should also 
install the commandline tools. Both of these can be found on 
[this page](https://developer.android.com/studio/index.html). After the Android SDK install you will need to open the 
"Android SDK Manager" and install Android platform 23, along with the "Android Support Repository".

There is one other thing you must make sure to do and that is to correctly set the `ANDROID_HOME` environment variable 
to be the root directory of where you have installed the Android commandline tools. This is because this projects build 
uses that environment variable to find the Android libraries it uses.

##### Install Android Platforms into local Maven repository

Clone the [maven-android-sdk-deployer](https://github.com/simpligility/maven-android-sdk-deployer) github project and 
build it with the following profile.
```bash
git clone https://github.com/simpligility/maven-android-sdk-deployer.git
cd maven-android-sdk-deployer/
mvn clean install -P6.0 
```
This build will only succeed if you have installed Android platform 23 in the step above. What this does is install all 
the base Android SDK libraries into your local maven repository.

### Build

Once you have carried out the prerequisites simple run a standard maven build.

```bash
mvn clean verify
```

This will build and package the Android APK.

### Deploy

To deploy the APK to the default device simply run.

```bash
mvn android:deploy
```

This is done with the [android-maven-plugin](https://simpligility.github.io/android-maven-plugin/), further build 
instructions can be found in it's documentation.

### Intellij

Intellij will open and configure this project correctly just like a Gradle project, just open it like any other Maven 
project.

Unfortunately it will not automatically create the default run configuration, you will have to add this manually, though 
this can be done by just adding an Android configuration that is set to the current module with all the other settings 
left as default.

### Troubleshooting

##### Intellij can't find the platform version

Sometime Intellij will complain that it can't find the Android platform you have specified, this seems to happen if you 
open the project with the platform version set to the highest version you have installed. So this will happen with this 
project if the highest platform version you have installed is 23. To fix this remove the whole `<sdk/>` tag from the 
`pom.xml` so that no platform version is explicitly configured. This will cause Intellij to choose the highest platform 
version by default and succeed to configure the project and add the Android SDK. Once Intellij finishes it's setup and 
indexing you can restore the `<sdk/>` tag with it's explicit platform version.

##### Intellij can't render the layout

When you first open the layout `activity_main.xml` file you may get an error stating that Intellij cannot find a 
`R$style` class which stops the layout from rendering. This can be fixed by rebuilding the project through Intellij with
"Build -> Rebuild Project", then closing and reopening the project.