
README
------

This bundle plugin is an example of how to add 3rd party jars to a bundle plugin.

How to build this example
-------------------------

To install all you need to do is build the JAR. to do this run 
./gradlew jar
This will build a jar in the build/libs directory

1. To install this bundle:

Copy the bundle jar file inside the Felix OSGI container (dotCMS/felix/load).
        OR
Upload the bundle jar file using the dotCMS UI (CMS Admin->Dynamic Plugins->Upload Plugin).
	
2. To uninstall this bundle:

Remove the bundle jar file from the Felix OSGI container (dotCMS/felix/load).
        OR
Undeploy the bundle using the dotCMS UI (CMS Admin->Dynamic Plugins->Undeploy).

How to create a bundle plugin with external jars
-------------------------------------------

--
In order to create this OSGI plugin, you must write the META-INF/MANIFEST
to be inserted into OSGI jar.

This file is being created for you by Gradle. If you need you can alter our config for this but in general our out of the box config should work.
The Gradle plugin uses BND to generate the Manifest. The main reason you need to alter the config is when you need to exclude a package you are including on your Bundle-ClassPath

In this MANIFEST you must specify (see template plugin):

Bundle-Name: The name of your bundle

Bundle-SymbolicName: A short an unique name for the bundle

Bundle-Activator: Package and name of your Activator class (example: com.dotmarketing.osgi.external.Activator)

Bundle-ClassPath: The Bundle-ClassPath specifies where to load classes from from the bundle.
                  This is a comma separated list of elements to load (such as current folder,lib.jar,other.jar).
                  (example: .,lib/date4j.jar )

DynamicImport-Package: *
    Dynamically add required imports the plugin may need without add them explicitly

Import-Package: This is a comma separated list of package's name.
                In this list there must be the packages that you are using inside
                the bundle plugin and that are exported by the dotCMS runtime.

Beware!!!
---------

In order to work inside the Apache Felix OSGI runtime, the import
and export directive must be bidirectional.

As of dotcms 2.5.2 if you do not start with a dotCMS/WEB-INF/felix/osgi-extra.conf ALL packages will be exported for you. 
So there is nothing for you to do


The DotCMS must declare the set of packages that will be available to
the OSGI plugins by changing the file: dotCMS/WEB-INF/felix/osgi-extra.conf.
This is possible also using the dotCMS UI (CMS Admin->Dynamic Plugins->Exported Packages).

Only after that exported packages are defined in this list,
a plugin can Import the packages to use them inside the OSGI blundle.

--
--
--
com.dotmarketing.osgi.external.Examples
-----------------------------------------------

Example class that will run simple examples using the 3rd party library that was added to the lib folder of the bundle (lib/date4j.jar).

--
Activator
---------

This bundle activator extends from com.dotmarketing.osgi.GenericBundleActivator and implements BundleActivator.start().
Calls the Examples class static methods to run simple tests that use the 3rd party library date4j.jar
