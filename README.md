
<A name="toc1-3" title="zproject - Class Project Generator" />
# zproject - Class Project Generator

<A name="toc2-6" title="Contents" />
## Contents


**<a href="#toc2-11">Overview</a>**
&emsp;<a href="#toc3-16">Scope and Goals</a>

**<a href="#toc2-46">Demo on PLAYTerm</a>**

**<a href="#toc2-51">Sample configuration</a>**

**<a href="#toc2-181">Sample API model</a>**

**<a href="#toc2-288">Installation</a>**
&emsp;<a href="#toc3-291">GSL</a>
&emsp;<a href="#toc3-303">zproject</a>

**<a href="#toc2-321">Setup your project environment</a>**

**<a href="#toc2-343">Ownership and License</a>**

**<a href="#toc2-352">Removal</a>**
&emsp;<a href="#toc3-355">autotools</a>
&emsp;<a href="#toc3-360">Hints to Contributors</a>
&emsp;<a href="#toc3-367">This Document</a>

<A name="toc2-11" title="Overview" />
## Overview

zproject is a community project, like most ZeroMQ projects, built using the C4.1 process, and licensed under MPL v2. It solves the Makefile problem really well. It is unashamedly for C, and more pointedly, for that modern C dialect we call CLASS. CLASS is the Minecraft of C: fun, easy, playful, mind-opening, and social. [hintjens#79](http://hintjens.com/blog:79)

<A name="toc3-16" title="Scope and Goals" />
### Scope and Goals

zproject has these goals:

* generate files for cross-platform build environments.
* generate public and private headers. 
* generate CLASS (ZeroMQ RFC/21) compliant header and source skeletons for new classes.
* generate CI setup for travis.

zproject grew out of the work that has been done to automatically generate the build environment in CZMQ.

All you need is a project.xml in the project's root directory which is your 

    One file to rule them all

The following build environments are currently supported:
 
* android (not tested)
* autotools (tested)
* cmake (not tested)
* mingw32 (not tested)
* qt-android (tested)
* vs2008 (not tested)
* vs2010 (not tested)
* vs2012 (not tested)
* vs2013 (not tested)
 
All classes in the project.xml are automatically added to all build environments. Further as you add new classes to your project you can generate skeleton header and source files according to [the CLASS RFC](http://rfc.zeromq.org/spec:21).

<A name="toc2-46" title="Demo on PLAYTerm" />
## Demo on PLAYTerm

[ZeroMQ - Create new zproject](http://www.playterm.org/r/zeromq---create-new-zproject-1424116766) 

<A name="toc2-51" title="Sample configuration" />
## Sample configuration

The following snippet is the `project.xml` from zproject:

<!-- 
    The project.xml generates build environments for:

        * android
        * autotool
        * cmake
        * mingw32
        * cygwin
        * vs2008
        * vs2010
        * vs2012
        * vs2013

    Classes are automatically added to all build environments. Further as you
    add new classes to your project you can generate skeleton header and source 
    files according to http://rfc.zeromq.org/spec:21.

    name := The name of your project
    description := A short description for your project
    script := The gsl script to generate all the stuff !!! DO NOT CHANGE !!!
    email := The email address where to reach you project e.g. mailinglist.
-->
<project
    name = "myproject"
    description = "My Project that will change the world."
    script = "zproject.gsl"
    email = "mail@myproject.org"
    >

    <!--
        Includes are processed first, so XML in included files will be
        part of the XML tree
    -->
    <include filename = "license.xml" />

    <!-- 
        Current version of your project. 
        This will be used to package your distribution 
    -->
    <version major = "1" minor = "0" patch = "0" />

    <!--
        Specify which other projects this depends on.
        These projects must be known by zproject, and the list of
        known projects is maintained in the zproject_known_projects.xml model.
        You need not specify subdependencies if they are implied.
    <use project = "zyre" min_major= "1" min_minor = "1" min_patch = "0" />
    -->

    <!-- Header Files
         name := The name the header file to include without file ending
    <header name = "myproject_prelude" />
    -->

    <!-- 
        Classes, if the class header or source file doesn't exist this will
        generate a skeletons for them.
        use private = "1" for internal classes
    <class name = "myclass">Public class description</class>
    <class name = "someother" private = "1">Private class description</class>
    -->

    <!--
        Main programs built by the project
                 use private = "1" for internal tools
    <main name = "progname">Exported public tool</main>
    <main name = "progname" private = "1">Internal tool</main>
    <main name = "progname" service = "1">Installed as system service</main>
    -->

    <!-- 
        Models that we build using GSL. 
        This will generate a 'make code' target to build the models.
    <model name = "sockopts" />
    <model name = "zgossip" />
    <model name = "zgossip_msg" />

        If a model should be generated using a specific gsl script,
        this can be set through the script attribute:
    <model name = "hydra_msg" script = "zproto_codec_java.gsl" />

        Additional parameters to the script can be set via nested
        param elements:
    <model name = "hydra_msg" script = "zproto_codec_java.gsl">
        <param name = "root_path" value = "../main" />
    </model>
    -->

    <!-- Other source files that we need to package
    <extra name = "some_ressource" />
    -->

    <!-- 
        Stuff that needs to be installed:
        
        * Linux -> /usr/local/bin
    -->
    <bin name = "zproject.gsl" />
    <bin name = "zproject_projects.gsl" />
    <bin name = "zproject_known_projects.xml" />
    <bin name = "zproject_class_api.gsl" />
    <bin name = "zproject_mkman" />
    
    <bin name = "zproject_android.gsl" />
    <bin name = "zproject_autoconf.gsl" />
    <bin name = "zproject_automake.gsl" />
    <bin name = "zproject_bindings_python.gsl" />
    <bin name = "zproject_bindings_qml.gsl" />
    <bin name = "zproject_bindings_ruby.gsl" />
    <bin name = "zproject_ci.gsl" />
    <bin name = "zproject_class.gsl" />
    <bin name = "zproject_cmake.gsl" />
    <bin name = "zproject_docs.gsl" />
    <bin name = "zproject_git.gsl" />
    <bin name = "zproject_lib.gsl" />
    <bin name = "zproject_mingw32.gsl" />
    <bin name = "zproject_cygwin.gsl" />
    <bin name = "zproject_qt_android.gsl" />
    <bin name = "zproject_tools.gsl" />
    <bin name = "zproject_vs2008.gsl" />
    <bin name = "zproject_vs2010.gsl" />
    <bin name = "zproject_vs2012.gsl" />
    <bin name = "zproject_vs2013.gsl" />
</project>

<A name="toc2-181" title="Sample API model" />
## Sample API model

The zproject scripts can also optionally generate the `@interface` in your class headers from an API model, in addition to a host of language bindings.  To opt-in to this behavior, just make a model to the `api` directory of your project.  For example, if your `project.xml` contains `<class name = "myclass"/>`, you could create the following `api/myclass.xml` file:

<!--
    This model defines a public API for binding. 
-->
<class name = "myclass" >
    My Feature-Rich Class

    <include filename = "../license.xml" />

    <constant name = "default port" value = "8080">registered with IANA</constant>

    <!-- Constructor is optional; default one has no arguments -->
    <constructor>
        Create a new myclass with the given name.
        <argument name = "name" type = "string" />
    </constructor>

    <!-- Destructor is optional; default one follows standard style -->
    <destructor />

    <!-- This models a method with no return value -->
    <method name = "sleep">
        Put the myclass to sleep for the given number of milliseconds.
        No messages will be processed by the actor during this time.
        <argument name = "duration" type = "integer" />
    </method>

    <!-- This models an accessor method -->
    <method name = "has feature">
        Return true if the myclass has the given feature.
        <argument name = "feature" type = "string" />
        <return type = "boolean" />
    </method>

    <!-- Callback typedefs can be declared like methods -->
    <callback_type name = "handler_fn">
        <argument name = "self" type = "myclass" />
        <argument name = "action" type = "string" />
        <return type = "boolean" />
    </callback_type>

    <!-- Callback types can be used as method arguments -->
    <method name = "add handler">
        Store the given callback function for later
        <argument name = "handler" type = "myclass_handler_fn" callback = "1" />
    </method>

    <!-- If singleton = "1", no class struct pointer is required. -->
    <method name = "test" singleton = "1">
        Self test of this class
        <argument name = "verbose" type = "boolean" />
    </method>

    <!-- These are the types we support
         Not all of these are supported in all language bindings;
         see each language binding's file for supported types in that
         language, and add more types as needed where appropriate.
         -->
    <method name = "tutorial">
        <argument name = "void pointer" type = "anything" />
        <argument name = "standard int" type = "integer" />
        <argument name = "standard float" type = "real" />
        <argument name = "standard bool" type = "boolean" />
        <argument name = "char pointer" type = "string" />
        <argument name = "custom pointer" type = "myclass_t" />
        <return type = "nothing">void method</return>
    </method>
</class>

This would cause the following `@interface` to be generated inside of `include/myclass.h`.  Note that if `include/myclass.h` has other handwritten content outside of the `@interface` this content will be retained.

    //  @warning THE FOLLOWING @INTERFACE BLOCK IS AUTO-GENERATED BY ZPROJECT!
    //  @warning Please edit the model at "api/myclass.xml" to make changes.
    //  @interface
    //  Create a new myclass with the given name.
    MYPROJECT_EXPORT myclass_t *
        myclass_new (const char *name);

    //  Destroy the myclass.
    MYPROJECT_EXPORT void
        myclass_destroy (myclass_t **self_p);

    //  Return true if the myclass has the given feature.
    MYPROJECT_EXPORT bool
        myclass_has_feature (myclass_t *self, const char *feature);

    //  Put the myclass to sleep for the given number of milliseconds.
    //  No messages will be processed by the actor during this time.  
    MYPROJECT_EXPORT void
        myclass_sleep (myclass_t *self, int duration);

    //  Self test of this class
    MYPROJECT_EXPORT void
        myclass_test (bool verbose);
    //  @end

Language bindings will also be generated in the following languages:

* Ruby (experimental)
* QML (experimental)

The language bindings are minimal, meant to be wrapped in a handwritten idiomatic layer later.

<A name="toc2-288" title="Installation" />
## Installation

<A name="toc3-291" title="GSL" />
### GSL

zproject uses the code generator called GSL to process its inputs and create its outputs. Before you start you'll need to install GSL (https://github.com/imatix/gsl) on your system. 

	git clone https://github.com/imatix/gsl.git
	cd gsl
	./autogen.sh
	./configure
	make
	make install

<A name="toc3-303" title="zproject" />
### zproject

You must then install the zproject resources into your system.

The following will install the zproject files to `/usr/local/bin` where gsl will find them.

	git clone https://github.com/zeromq/zproject.git
	cd zproject
    ./autogen.sh
    ./configure
    make
    make install

NB: You may need to use the `sudo` command when running `make install` to elevate your privileges, e.g.

	sudo make install

<A name="toc2-321" title="Setup your project environment" />
## Setup your project environment

Copy the `project.xml` and `generate.sh` to your project or an empty directory and adjust the values accordingly. You'll also need a license file. To get started you can copy `license.xml` from zproject and change the license to whatever you like. The text in the `license.xml` will be placed on every header and source file. Thus make sure not to insert the hole license but a appropriate disclaimer.

	mkdir myproject
	cd myproject
	cp ~/zproject/project.xml .
	cp ~/zproject/license.xml .
	cp ~/zproject/generate.sh .

Next, edit `project.xml` to your liking, then when you are ready to create/update you project

	./generate.sh
	./autogen.sh
	./configure.sh
	make

To also build the tests (assuming you have added some), use:

	make check

<A name="toc2-343" title="Ownership and License" />
## Ownership and License

The contributors are listed in AUTHORS. This project uses the MPL v2 license, see LICENSE.

zproject uses the [C4.1 (Collective Code Construction Contract)](http://rfc.zeromq.org/spec:22) process for contributions.

To report an issue, use the [zproject issue tracker](https://github.com/zeromq/zproject/issues) at github.com.

<A name="toc2-352" title="Removal" />
## Removal

<A name="toc3-355" title="autotools" />
### autotools

    make uninstall

<A name="toc3-360" title="Hints to Contributors" />
### Hints to Contributors

Before you commit code please make sure that the project model hides all details of backend scripts.

For example don't make a user enter a header file because autoconf needs it to do AC_CHECK_LIB.

<A name="toc3-367" title="This Document" />
### This Document

This document is originally at README.txt and is built using [gitdown](http://github.com/imatix/gitdown).
