simple packaging for openvas-manager

I made this, because I couldn't find anything else. I am not
a debian developer, there may be bugs, etc. But you're welcome
to use it, modify it, extend it

I built it from svn revision 7230 on Ubuntu Hardy 8.04
so that's what I know works. I expect other things work as well.

Note that the openvas suite of things has several poorly documented
interdependancies. I recommend against trying to mix versions.

You Milage May Vary

##########################################

# How I build packages:

# First, set up the build directory and checkout

mkdir /var/tmp/openvas-manager
cd /var/tmp/openvas-manager

svn checkout http://svn.wald.intevation.org/svn/openvas/trunk/openvas-manager/
cd openvas-manager
git clone \
    http://github.com/directionless/debian-packaging-openvas-manager.git debian

# You might need this patch to allow an out-of-source build.
# Or maybe it's been accepted upstream.
patch < debian/patches/cmake-out-of-source.txt

# If you're using a newer svn revision, you should update the
# debian/changelog
mkdir b
cd b
cmake ..
cd ..
dch --newversion $(cat b/VERSION) update to svn
perl -pi -e 's{jaunty}{UNRELEASED}' debian/changelog
rm -rf b

# If you've updated control.in, you'll need to regenerate control.
# You probably don't need this.
# ./debian/rules debian/control DEB_AUTO_UPDATE_DEBIAN_CONTROL=yes

# Build the package
debuild -i -us -uc -b

# Enjoy. It should be in ../



