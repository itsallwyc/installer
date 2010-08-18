Tool Installation

(from http://www.pion.org/files/pion-platform/common/doc/README.msvc)

To install Visual C++ 2005 Express Edition and the Microsoft Platform
SDK, follow the steps described at the following URL:

http://www.microsoft.com/express/2005/platformsdk/default.aspx

SDK at http://www.microsoft.com/downloads/details.aspx?displaylang=en&FamilyID=0baf2b35-c656-4969-ace8-e4c0c0716adb

In order for the Visual Studio Command Line Compiler to find the necessary
Microsoft Platform SDK files, one more file must be modified: open
"C:\Program Files\Microsoft Visual Studio 8\VC\vcvarsall.bat" in an editor
and add the following lines at the top, after "@echo off":

@set PATH=C:\Program Files\Microsoft Platform SDK for Windows Server 2003 R2\Bin;%PATH%
@set INCLUDE=C:\Program Files\Microsoft Platform SDK for Windows Server 2003 R2\Include;%INCLUDE%
@set LIB=C:\Program Files\Microsoft Platform SDK for Windows Server 2003 R2\Lib;%LIB%

====


Here is the script that I use to produce the installer.

It is invoked like this:

  > build-installer.py --build-libs --zip --build-installer \
    --build-tools <--dvd> [ABSOLUTE_PATH_TO_BOOST]

for instance, this produced the 1.39 installer:

  > build-installer.py --build-libs --zip --build-installer \
    --build-tools e:\boost\boost_1_39_0

After this is done you need to upload the binaries. I do this with
something like:

  @boostpro $ mkdir /home/daniel/1_39

  > cd 1_39-installer/zips
  > scp *.zip daniel@boostpro.com:1_39

and then move the binaries on the server:

 @boostpro $ mv /home/daniel/1_39 \
   /usr/local/www/data/boost-consulting/boost-binaries/

and set up the mirrors.txt file for the release:

 @boostpro $ echo "BoostPro Computing" >> \
   /usr/local/www/data/boost-consulting/boost-binaries/1_39/mirrors.txt

 @boostpro $ echo "http://www.boostpro.com/boost-binaries/1_39/" >> \
   /usr/local/www/data/boost-consulting/boost-binaries/1_39/mirrors.txt

You need the NSIS ZipDLL, NXS, and inetc plugins.  Drop the DLLs into
your NSIS Plugins folder.  Then Run NSIS on the generated .nsi file by 
right-clicking on it and selecting "compile NSIS file."

Now you should have a working installer. After this it's possible to
upload the binaries to sourceforge, and add mirrors to the mirrors.txt
file, but the procedure for that has changed. It's pretty simple though,
you can rsync (or something similar) the files from the server to SF.

New libraries need to be added to "lib-names.txt".

There are a few manual steps as you can see, but they take me like 5
minutes to perform so I didn't bother scripting them.

Of course, you can just do the building if you want to and I can do the
manual steps when the build is done. For me, the build step takes more
than 24 hours, so it locks up my work station during that time. I also
sometimes have to do it incrementally because of lack disk space.

Oh, you also need 7-zip installed. A standard install should just work.
