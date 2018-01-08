# Steps
to rename a folder on android(connected to firebase)

**Aim: change the project name from com.example.androis.firebase3in1 to com.work.chaostools.jimsadmin 
(a projectname which should not conflict with google's  guidelines regarding  playstore app-project names)
and project name from "Firebase 3 in 1" to "Jims- Admin"**

- **rename package(not directory)**:click on package name
- **FIREBASE PART**: you basically want to do step 3 but could not unless the google services.json is
  updated . so click on tools>firebase>connect to firebase(choose any of the options's 1st step)
  
- **application id**:now change the id Using file>project structure> flavours >id. change the id as per
  the package name.

- **APP NAME change:** this could have been the 1st and only step, but this will just change the app
 and notthe complete app id(google will still reject your appp from play store if its id starts
 with com.example....)
 anyways goto manifest and renampe Appname

- **final change**: close your app, go to the place where all your projects are stored , via your file
  explorer.Manually rename your outermost package name
