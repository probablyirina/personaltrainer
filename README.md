[![Build Status](https://travis-ci.org/probablyirina/personaltrainer.svg?branch=master)](https://travis-ci.org/probablyirina/personaltrainer)

## Personal trainer
Spring Boot/Data/Security, REST, Maven, Hibernate, Flyway, Grunt, Bower, Knockout, Requirejs app integrated with [Travis CI](https://travis-ci.org/) and deployed on [Heroku](https://www.heroku.com/) cloud.

###Dev setup
To start, you will need Java 7 or 8 installed. Project has been developed and tested on Linux; it might work on Windows but you are on your own.  
Install [Git] (https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), [Maven](https://maven.apache.org/), and [npm](https://www.npmjs.com/).  
Project has separate frontend and backend builds integrated by using [grunt-maven-plugin] (https://github.com/allegro/grunt-maven-plugin). 

###Front-end build
Js libraries are managed by [Bower](http://bower.io/). Bower dependencies are defined in [*bower.json*](https://github.com/probablyirina/personaltrainer/blob/master/bower.json) file. 
Dependencies are downloaded and installed during Maven build phase into a dir specified in [*.bowerrc*](https://github.com/probablyirina/personaltrainer/blob/master/.bowerrc) conf file.  
Front-end sources are located under [WEB-INF](https://github.com/probablyirina/personaltrainer/tree/master/src/main/webapp/WEB-INF) dir (because when you forget to exclude them from WAR, they cannot be accessed by the Spring).  
Front-end build process in managed by [Grunt](http://gruntjs.com/) and its plugins. File [*package.json*](https://github.com/probablyirina/personaltrainer/blob/master/package.json) defines all Grunt modules. 
Grunt and Bower are installed locally during Maven build task; Grunt creates *node_modules*, while Bower downloads libraries and puts them in *src/main/webapp/WEB-INF/js/lib* dir as specified in .bowerrc configuration.  

###Grunt-maven-plugin configuration
Grunt-maven-plugin default configuration is modified as you can see in [Maven configuration](https://github.com/probablyirina/personaltrainer/blob/master/pom.xml).  
Output artifacts are created in *src/main/webapp/static* (as opposed to default *target-grunt* dir). Spring Boot project is run by invoking *spring-boot:run*, which runs the app extracted rather than from WAR, 
and expects the resources to be under *src/main/webapp* (Spring Boot looks also in *static* or *public* subdirectories inside *resources*). Front-end source files are in *src/main/webapp/WEB-INF*, because if you forget to exclude them from WAR, 
they cannot be accessed by the Spring MVC servlet. 
The grunt-maven-npm workflow is usually completed by using mavenDist Grunt task. It's purpose is to copy the contents of a directory which has all output artifacts generated by Grunt tasks to project's target directory. 
This way they would be packaged into the resulting WAR once the Maven buildcycle completes.  
Grunt itself handles copying files into *static* dir (in its copy task), thus removing a need for mavenDist Grunt task, and an additional configuration file. This is done by removing create-resources goal from the plugin config.
Package.json, bower.json and Gruntfile.js are kept in project's root where pom.xml is, which makes them easier to find for humans and IDE plugins. The gruntBuildDirectory property is set to project's root for that to work.

###CI setup
Setup for cloud continuous integration is in [*.travis.yml](https://github.com/probablyirina/personaltrainer/blob/master/.travis.yml) file. On commit to the master branch, Travis runs the tests and automatically deploys a successful build to Heroku.

###How to build and run the project
Spring Boot project is run by running  
```mvn spring-boot:run```  
from the project dir. You should see the app running at *localhost:8181*. To change server port, modify setting in [application.properties] (https://github.com/probablyirina/personaltrainer/blob/master/src/main/resources/application.properties). 
