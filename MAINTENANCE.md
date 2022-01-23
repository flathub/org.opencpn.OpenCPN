Making a new release
--------------------

   1. Clone this repo
   2. Checkout correct branch, _stable_ or _beta_
   3. Patch the manifest to refer to the proper OpnCPN commit, something
      like
```
        --- a/org.opencpn.OpenCPN.yaml
        +++ b/org.opencpn.OpenCPN.yaml
        @@ -122,7 +122,7 @@ modules:
                   - type: git
                     url: https://github.com/OpenCPN/OpenCPN.git
                     # branch: master
        -            commit: 96e29a4
        +            tag: 0.5.6-beta1
                     disable-fsckobjects: true
                   - type: patch
                     path: 0004-flatpak-Add-a-shell-wrapper.patch
```
   4. Commit and push the change
   5. Go to https://flathub.org/builds. The build starts automatically
      after the push and takes around 15 minutes.
   6. Using the login drop-down top-right, login using "Login with Github"
   7. If the build fails, it must be fixed... edit, commit and push again.
      Build logs are available after pushing the leftmost, 5-digit build
      number button
   8. When the build is OK, the build number button becomes green. Push it.
   9. The view of the build contains
        - A link to the test repo at bottom. Make a smoke test to make sure
          it is sane
        - Three buttons top-right Rebuild, Publish, Delete. If the build is
          OK, push the Publish button, otherwise Delete.
      NOTE: If nothing is done, the test build is eventually automatically
      published after around a week (200 hours).
  10. Please update this document as required.


Making a local build
--------------------
Building requires up to date flatpak-builder. Such is available on Ubuntu
Focal+ and recent Fedora like F32+. Updated flatpak-builder packages for
Ubuntu are available at
https://launchpad.net/~alexlarsson/+archive/ubuntu/flatpak.

The build:

  1. Clone this repository
  2. Check out correct branch: _stable_ or _beta_
  3. Make sure the repo is clean: commit or stash all changes.
  4. Update the shared\_modules submodule:

         $ git submodule init shared-modules
         $ git submodule update shared-modules

  5. If the submodule update ended up in a change visible using
     `git diff`, commit it.
  6. Locate the runtime version used in the org.opencpn.OpenCPN.yaml 
     manifest. At the time of writing, this is 20.08.
  7. Using the correct runtime version, install the build dependencies:

         $ flatpak install  org.freedesktop.Sdk//20.08

  8. Build using

        $ flatpak-builder --repo=repo --force-clean --default-branch=devel \
          app org.opencpn.OpenCPN.yaml 

  9. Test using

        $ flatpak install -y --user --reinstall repo org.opencpn.OpenCPN
        $ flatpak run org.opencpn.OpenCPN//devel