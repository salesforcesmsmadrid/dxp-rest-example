###Summary
This project provides an example of how to build JAX-RS based REST services in Liferay DXP. It demonstrates two REST resources:
- A simple GET service that returns a transformed/abbreviated representation of the currently logged in user in JSON
- A full in-memory CRUD implementation (GET, PUT, POST, DELETE) for a hypothetical set of Person objects. This is the same sample class that is included as a part of the Xtivia Services Framework (XSF) which has been ported to use JAX-RS

This project builds off of the original example created by Ray Auge of Liferay and adds the following aspects:

- Is self-contained in a single Gradle project/build (no parent project dependencies)
- Demonstrates how to write a function that runs when the bundle is deployed/activated in the OSGi container
- Shows how to include multiple REST resources (e.g. /users, /people) in a single application (OSGi bundle)
- Leverages the Jackson JAX-RS content handler for simplified conversion of Java POJOs to/from JSON
- Includes a simple technique in the Gradle build file for automatically including dependent JARs in both the OSGi classpath and in the packaged OSGi bundle (JAR), i.e., manual editing of a bnd.bnd file is not required as new dependencies are added to the build script


###Prerequisites

- Install Gradle from http://gradle.org/gradle-download. (**NOTE**: this project's build script uses features introduced in Gradle 2.12 so that is the **minimum** required version).

- Install and configure Liferay DXP or Liferay 7 CE.

###Build

Clone this repo and then cd into the project directory containing build.gradle and run 'gradle build'

###Deploy

Copy the JAR file created by the build step from the [projectdir]/build/libs directory and drop it into the 'deploy' directory for your DXP installation.

###DXP Configuration

NOTE: Before these endpoints are accessible, one needs to configure DXP endpoints for it using the OSGi configuration interface provided in DXP. To do so, go to Control Panel > System > System Settings > Foundation and then ...

    Search for CXF Endpoints
    Create new CXFEndpoint publisher configuration providing value for Context Path (/samples)
    Go back to System Settings > Foundation and select REST Extender
    Create new Rest Extender configuration (search with rest) providing Context Path (/samples)

Then you can access the services as described below:

###REST Usage

If all of the steps above have been successfully executed you should now have access to the following REST endpoints (assuming that your local copy of DXP is accessible at http://localhost:8080). **NOTE:** the **/o** portion of the URLs below is required; this is the URL mapping associated with the DXP servlet that services all requests targeted to the OSGi container.

| URL   |      Method      |  Notes |
|----------|:-------------:|:------|
| http://localhost:8080/o/samples/users/current |GET| Returns a simplified JSON representation of Liferay User object for logged in user (or default user if not logged in) |
| http://localhost:8080/o/samples/people |GET| Returns the entire collection of in-memory Person objects   |
| http://localhost:8080/o/samples/people/{id} | GET |  Returns a single Person object based on suppplied ID   |
| http://localhost:8080/o/samples/people | POST |Creates a new Person object based on suppplied JSON and returns the created object including ID      |
| http://localhost:8080/o/samples/people/{id} | PUT |Modifies an existing Person object based on suppplied JSON and uses the ID parameter to identify the target person     |
| http://localhost:8080/o/samples/people/{id} | DELETE |  Removes a single Person object based on suppplied ID   |