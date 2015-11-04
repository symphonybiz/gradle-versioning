# Gradle versioning example for an Android app

This is an example Gradle script for handling automatic versioning for an Android app based on Git tags.

`versioning.info` is the script file that performs version parsing and provides an API
Include it by specifying `apply from: 'versioning.gradle'` in your main `build.gradle` file

Create first a Git tag in your branch with the following format:
`git tag -a v1.4.2 -m 'my version 1.4.2'`

Call `git tag` to check that you created it correctly

In your `build.gradle` file you can use the script to:
* Automatically generate Android version name and version:
    versionCode getAndroidVersionCode()
    versionName getAndroidVersionName()

* Save version information in Android BuildConfig constants, and access them at run time in the app:
    buildConfigField "String", "GIT_SHA", "\"${getGitSha()}\""
    buildConfigField "String", "BUILD_TIME", "\"${getBuildTime()}\""
    buildConfigField "String", "FULL_VERSION_NAME", "\"${getVersionName()}\""
    buildConfigField "String", "VERSION_DESCRIPTION", "\"${StringEscapeUtils.escapeJava(getVersionInfo())}\""

* Save a `build.info` file containg version information:
    task saveBuildInfo {
      def buildInfo = getVersionInfo()
      def assetsDir = android.sourceSets.main.assets.srcDirs.toArray()[0]
      assetsDir.mkdirs()
      def buildInfoFile = new File(assetsDir, 'build.info')
      buildInfoFile.write(buildInfo)
    }
    gradle.projectsEvaluated {
      assemble.dependsOn(saveBuildInfo)
    }


Check out the `build.gradle` file in the `app` folder for an example of using the versioning API

Enjoy!
