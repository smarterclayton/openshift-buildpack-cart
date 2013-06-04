OpenShift Buildpack Cartridge
=============================

!!! DOES NOT YET LAUNCH PROCESSES, STILL WORKING ON THAT !!!

Supports the [Heroku buildpack API](https://devcenter.heroku.com/articles/buildpack-api) on OpenShift.  Deploy a new app with:

    $ rhc create-app myapp "http://cartreflect-claytondev.rhcloud.com/reflect?github=smarterclayton/openshift-buildpack-cart"

then set the BUILDPACK environment variable inside the gear:  
    
    $ rhc ssh myapp
    Connected to ...
    $> echo "<buildpack_url>" > buildpack/env/BUILDPACK_URL
    $> exit
    
and finally check your code in to the created git repository and push it to OpenShift

    $ cd myapp
    ...
    $ git add . && git commit -m "First commit"
    $ git push origin master


Caveats
-------

Heroku and OpenShift are running different Linux operating systems (Ubuntu vs. RHEL) and have different libraries and versions of libraries installed.  Buildpacks that depend on specific behavior of the Heroku runtime environment may not work on OpenShift, such as:

* Binaries compiled with Vulcan or checked into the buildpack repository may not have the same local dependencies or architectures
* Python 2.7 is not the default Python version inside a gear (instructions TBD on how to best configure this)
* Some system executables are not in the /usr/local/bin directory, but instead in /usr/bin or /usr/sbin
* The default Ruby version is currently 1.8.7, and Gem installs will fail because ~/.gem is not writable

I hope to address some of these over time.


TODOs
-----

* Implement Foreman process support
* Make Python 2.7 and Ruby 1.9 the default runtime environments in buildpack gears
* Get environment variable support in upstream so buildpack can be set on creation
* Allow gems to be installed at the gear level
