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

Ant - Data Types
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

