I created this hello world app like so:
1. Per the instructions here: https://capacitor.ionicframework.com/docs/getting-started/with-ionic/  
I created the app using the below commands:  
`ionic start myApp tabs --capacitor`  
(I chose the angular option for 'framework')  
`ionic build`  
`npx cap add android`  

2. Created a git repo for the project

3. Per the instructions here: https://ionicframework.com/blog/introducing-capacitor-support-for-ionic-appflow/  
I add the AppFlow feature using the below commands:  
`ionic link`  
(First prompt: Create new app or link existing one?  I chose "create a new app")  
(Second prompt: provide a name for new app)  
(Third prompt: Link to github and selected the repository that I created in step 2, and linked to master branch)  
`ionic deploy add`  
(First prompt: Appflow App ID?  I just clicked enter to use the created app in the previous command)  
(Second prompt: Channel Name?  "Master")  
(Third Prompt: Update Method?  "background")  

4. I committed the code to the repo  

5. I set gradlew permissions for build:  
`cd android`  
`git update-index --chmod=+x gradlew`  

5. Update version of gradle being used in build.gradle (root)  
` classpath 'com.android.tools.build:gradle:3.6.3'`  (vs 3.6.1)  
*A note that I chose to update this because when I built locally in android studio, it wanted to update the gradle version.  I attempted an appflow build using 3.6.1 and 3.6.3 and both ended up with the error at the bottom.*

6. Committed code 

7. Logged into ionic appflow dashboard (https://dashboard.ionicframework.com/) and built the latest commit for android / debug 

8. Downloaded apk and installed on device to verify it does not crash

9. I navigated to the project root and installed cordova-background-geolocation-lt per here: https://github.com/transistorsoft/cordova-background-geolocation-lt/blob/master/help/INSTALL_CAPACITOR.md  
I used the below commands    
`npm install cordova-background-geolocation-lt`  
`npx cap sync`  

10. I added the following lines to app/build.gradle
```diff
apply plugin: 'com.android.application'

+def background_geolocation = "../../node_modules/cordova-background-geolocation-lt/src/android"
+apply from: "$background_geolocation/app.gradle"

android {
    .
    .
    .
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
+           proguardFiles "$background_geolocation/proguard-rules.pro"
        }
    }
}
```

11. Commit to AppFlow

12. Log into ionic dashboard and built latest commit for android in debug mode

13. Build fails with: 
* What went wrong:
A problem occurred evaluating project ':capacitor-cordova-android-plugins'.
Could not get unknown property 'applicationVariants' for object of type com.android.build.gradle.LibraryExtension.

![image](https://user-images.githubusercontent.com/6031711/81096998-3b29e100-8ed5-11ea-87ef-277716d81132.png)

