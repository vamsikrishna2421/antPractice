# Apache Ant Tasks
Ant - Property Task
1 ant.file:The full location of the build file.
2 ant.version:The version of the Apache Ant installation.
3	basedir:The basedir of the build, as specified in the basedir attribute of the project element.
4	ant.java.version:The version of the JDK that is used by Ant.
5	ant.project.name:The name of the project, as specified in the name atrribute of the project element.
6	ant.project.default-target:The default target of the current project.
7	ant.project.invoked-targets:Comma separated list of the targets that were invoked in the current project.
8	ant.core.lib:The full location of the Ant jar file.
9	ant.home:The home directory of Ant installation.
10	ant.library.dir:The home directory for Ant library files - typically ANT_HOME/lib folder.

ex:
<project name = "Hello World Project" default = "info">
   <target name = "info">
      <echo>Apache Ant version is ${ant.version}</echo>
   </target>
</project>

build.xml file and its associated build.properties file
<property file = "build.properties"/>

## Ant - Data Types
1.The fileset data types represents a collection of files. It is used as a filter to include or exclude files that match a particular pattern.
<fileset dir = "${src}" casesensitive = "yes">
   <include name = "**/*.java"/>
   <exclude name = "**/*Stub*"/>
</fileset>
? − Matches one character only.
* − Matches zero or many characters.
** − Matches zero or many directories recursively.

The following example depicts the usage of a pattern set.
<patternset id = "java.files.without.stubs">
   <include name = "src/**/*.java"/>
   <exclude name = "src/**/*Stub*"/>
</patternset>
The patternset can then be reused with a fileset as follows −
<fileset dir = "${src}" casesensitive = "yes">
   <patternset refid = "java.files.without.stubs"/>
</fileset>

2.The filelist data type is similar to the file set except the following differences −
filelist contains explicitly named lists of files and it does not support wild cards.
filelist data type can be applied for existing or non-existing files.
<filelist id = "config.files" dir = "${webapp.src.folder}">
   <file name = "applicationConfig.xml"/>
   <file name = "faces-config.xml"/>
   <file name = "web.xml"/>
   <file name = "portlet.xml"/>
</filelist>

3.Filter set
Using a filterset data type along with the copy task, you can replace certain text in all files that matches the pattern with a replacement value.
A common example is to append the version number to the release notes file, as shown in the following code.

<copy todir = "${output.dir}">
   <fileset dir = "${releasenotes.dir}" includes = "**/*.txt"/>
   
   <filterset>
      <filter token = "VERSION" value = "${current.version}"/>
   </filterset>
</copy>

Path
The path data type is commonly used to represent a class-path. Entries in the path are separated using semicolons or colons. However, these characters are replaced at the run-time by the executing system's path separator character.
The classpath is set to the list of jar files and classes in the project, as shown in the example below.

<path id = "build.classpath.jar">
   <pathelement path = "${env.J2EE_HOME}/${j2ee.jar}"/>
   
   <fileset dir = "lib">
      <include name = "**/*.jar"/>
   </fileset>
</path>


Documentation is a must in any project. Documentation plays a great role in the maintenance of a project. Java makes documentation easier by the use of the in-built javadoc tool.
<target name = "generate-javadoc">
   <javadoc packagenames = "faxapp.*" sourcepath = "${src.dir}" 
      destdir = "doc" version = "true" windowtitle = "Fax Application">
      
      <doctitle><![CDATA[= Fax Application =]]></doctitle>
      
      <bottom>
         <![CDATA[Copyright © 2011. All Rights Reserved.]]>
      </bottom>
      
      <group title = "util packages" packages = "faxapp.util.*"/>
      <group title = "web packages" packages = "faxapp.web.*"/>
      <group title = "data packages" packages = "faxapp.entity.*:faxapp.dao.*"/>
   </javadoc>

   <echo message = "java doc has been generated!" />
</target>
When the javadoc target is executed, it produces the following outcome −

C:\>ant generate-javadoc
Buildfile: C:\build.xml

java doc has been generated!

BUILD SUCCESSFUL
Total time: 10.63 second

The java documentation files are now present in the doc folder.
Typically, the javadoc files are generated as a part of the release or package targets.


## Ant - Creating JAR files
1  basedir:The base directory for the output JAR file. By default, this is set to the base directory of the project.
2	compress:Advises Ant to compress the file as it creates the JAR file.
3	keepcompression:While the compress attribute is applicable to the individual files, the keepcompression attribute does the same thing, but it applies to the entire archive.
4	destfile:The name of the output JAR file.
5  duplicate:Advises Ant on what to do when duplicate files are found. You could add, preserve, or fail the duplicate files.
6	excludes:Advises Ant to not include these comma separated list of files in the package.
7	excludesfile:Same as above, except the exclude files are specified using a pattern.
8	inlcudes:Inverse of excludes.
9	includesfile:Inverse of excludesfile.
10	update:Advises Ant to overwrite files in the already built JAR file.

<target name = "build-jar">
   <jar destfile = "${web.dir}/lib/util.jar"
      basedir = "${build.dir}/classes"
      includes = "faxapp/util/**"
      excludes = "**/Test.class">
      
      <manifest>
         <attribute name = "Main-Class" value = "com.tutorialspoint.util.FaxUtil"/>
      </manifest>
   </jar>
</target>
The following outcome is the result of running the Ant file −

C:\>ant build-jar
Buildfile: C:\build.xml

BUILD SUCCESSFUL
Total time: 1.3 seconds

## Ant - Creating WAR files
The WAR task is an extension to the JAR task, but it has some nice additions to manipulate what goes into the WEB-INF/classes folder, and generating the web.xml file. The WAR task is useful to specify a particular layout of the WAR file.

Since the WAR task is an extension of the JAR task, all attributes of the JAR task apply to the WAR task.
1	webxml:Path to the web.xml file
2	lib:A grouping to specify what goes into the WEB-INF\lib folder.
3	classes:A grouping to specify what goes into the WEB-INF\classes folder.
4	metainf:Specifies the instructions for generating the MANIFEST.MF file.

