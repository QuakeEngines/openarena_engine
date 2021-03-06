Unofficial port of OpenArena 0.8.8 client/server to the latest ioquake3
=======================================================================

This is based off of r28 of the binary thread which was used in the release
client/server for OpenArena 0.8.8.

It is intended to be as close as possible to 0.8.8 except when it makes
sense to deviate.

OpenArena 0.8.8 uses r1910 ioquake3 code.  This code currently targets
r2336 which is the latest.

Switching renderers
-------------------

Recent ioquake3 versions allow for a modular renderer.  This allows you to
select the renderer at runtime rather than compiling in one into the binary.

This feature is enabled by default.  If you wish to disable it, uncomment
USE_RENDERER_DLOPEN=0 in Makefile.local.

When you start OpenArena, you can pass the name of the dynamic library to
load.  ioquake3 assumes a naming convention renderer_*_.

Example:

    # Enable the default ioquake3 renderer.
    $ ./openarena.i386 +set cl_renderer opengl1

    # Enable the OpenArena renderer with GLSL, bloom support and more.
    $ ./openarena.i386 +set cl_renderer openarena1

    # Enable Rend2 (formerly renderergl2) which is now in upstream
    # Don't try this yet since OpenArena doesn't work with rend2 yet
    #$ ./openarena.i386 +set cl_renderer rend2

Building
--------

This a standard ioquake3 build which they describe here:

http://wiki.ioquake3.org/Building_ioquake3

It's not an autotools based build.  If you don't have the dependencies, it
will break in the middle of the build.

If you are on Ubuntu or Debian, the easiest way to compile this is to install
the build dependencies for the "ioquake3" package.

    $ sudo aptitude build-dep ioquake3
    $ git clone git://github.com/undeadzy/openarena_engine.git
    $ cd openarena_engine
    $ make

You may want to change a few things in Makefile.local.  Other than installing
the build dependencies, you shouldn't need to do anything else.  By default it
builds a modular renderer so you need the binary + *.so or *.dll files in the
same directory to run it.

Development
-----------

    # Get this project
    $ git clone git://github.com/undeadzy/openarena_engine.git
    $ cd openarena_engine

    # Create a reference to the upstream project
    $ git remote add upstream git://github.com/undeadzy/ioquake3_mirror.git

    # View changes in this project compared to ioquake3's SVN
    $ git fetch upstream
    $ git diff upstream/master

Changes from 0.8.8 release
--------------------------

* Sync with upstream so openarena.i386 uses the base ioquake3, openarena_rend2.i386
  uses Canete's Rend2 and openarena_oa.i386 uses the OA 0.8.8 renderer
  (plus minor updates).
* OA's renderer is now in renderer_oa and the base ioquake3 renderer is left
  untouched
* This does not remove the game or cgame files.  They are never referenced or
  built.  This makes it easier to keep in sync with upstream.  It would also
  be possible to integrate the OA engine and OA game code at some point in the
  future.
* Makefile has fewer changes since the recent upstream Makefile makes it easier
  to support standalone games
* cl_yawspeed and cl_pitchspeed are CVAR_CHEAT instead of removing the
  variables and using a constant.
* r_aviMotionJpegQuality was left untouched
* Enables STANDALONE so the CD key and authorize server related changes are
  no longer needed.
* Enables LEGACY_PROTOCOL and sets the version to 71.
* This uses a different GAMENAME_FOR_MASTER than 0.8.8.  It also uses the new
  HEARTBEAT_FOR_MASTER name since the code says to leave it unless you have a
  good reason.
* Any trivial whitespace changes were left out
* James Canete's renderer is now in upstream so it's in this project too.
  - It doesn't work yet with OA though
* GrosBedo added win32 support back to the Makefile

TODO
----

* Try to avoid changing qcommon area to support GLSL.  Canete's opengl2
  didn't need this change so this renderer shouldn't either.
* Remove compiler warnings.  I kept them in for now so the code would be
  as close as possible to 0.8.8.
* Verify that allowing say/say_team to bypass Cmd_Args_Sanitize is safe.
* Build in FreeBSD
* Build in MacOSX
* Cross compile in Linux for Windows
  - Cross compiling for Windows may require more changes to the Makefile to
    enable ogg vorbis support
* vm_powerpc_asm.c includes q_shared.h but upstream doesn't.  Why is this
  change in 0.8.8?
* Needs more testing
* Verify changes with OpenArena developers
* Potential GLSL debugging fix that was made available after 0.8.8 release.

Original file
-------------

OpenArena 0.8.8 client/server release source code:

http://files.poulsander.com/~poul19/public_files/oa/dev088/openarena-engine-source-0.8.8.tar.bz2

    $ sha1sum openarena-engine-source-0.8.8.tar.bz2
    64f333c290b15b6b0e3819dc120b3eca2653340e  openarena-engine-source-0.8.8.tar.bz2

