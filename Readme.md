# How to create a gradle library and publish it on Nexus

For any medium or large Java project we must package some of the classes into jars and use them all over the project microservices. In order to do that we use Nexus as a maven repository. 
In this short story we will point the basic steps to create a Gradle jar library and publish it on Nexus.

This is the repository for the story:
    - https://medium.com/@simionrazvan/how-to-create-a-gradle-library-and-publish-it-on-nexus-34be19b520aa

# Install Gradle

 

The first thing to do is to have the Gradle binaries in your operating system (we use Windows for this story). 


Download gradle binaries using the documentation located at https://gradle.org/install/ 

 

Extract zip content in a easy to use location, for example "C:\ProgramFiles\gradle"


Add the Gradle "bin" location path to System PATH Environment variable
 

Test the gradle "installation" using the common Windows terminal:

gradle --version


# Create a gradle wrapper

In order to ease the life of the programming team, we embed a Gradle wrapper in each Gradle project. 
Create the project folder
mkdir gradle-lib-nexus
cd gradle-lib-nexus
gradle wrapper --gradle-version 5.3.1 --distribution-type all

 

Now we can test the generated Gradle Wrapper:

gradlew –version

 

# Create the gradle library

We will use gradle init in order to generate a basic gradle library.
gradlew init --type java-library --project-name finance

 

We will open the project and see what’s been generated.

 

# Build the project

We can test the generated project by building it.
gradlew clean build

 

If you check the build/libs location you can find the generated library
 

# Publish the library to Nexus repository

We must do some preparations in order to publish the library in Nexus
1.	Create the gradle.properties file
repoUser=[your nexus user]
repoPassword=[your nexus password]



2.	Check gradle settings file
rootProject.name = 'finance'
 

3.	Update build.gradle file	
Add the following content:

version = '0.0.1'
group = 'ro.trc.common'
sourceCompatibility = '1.8'

apply plugin: 'maven-publish'
publishing {
    publications {
        maven(MavenPublication) {
            artifact("build/libs/finance-$version"+".jar") {
                extension 'jar'
            }
        }
    }
    repositories {
        maven {
            name 'nexus'
            url "http://repository.trencadis.ro:8081/repository/maven-releases/"
            credentials {
                username project.repoUser
                password project.repoPassword
            }}
    }
}

 
# Run the publish task

gradlew clean build publish



