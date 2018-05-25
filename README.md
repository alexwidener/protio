Protio
========
Premiere OpenTimelineIO extension. Pronounced "Pro-T-Oh".

Allows you to import and export OpenTimelineIO sequences, and launch the 

Installation
=============
##### Building requirements:

 - npm or yarn (yarn is in the make file, change it to npm if you need to)
 - gulp
 - jspm
 - make
 - On Windows, cygwin. Because why the fuck would you use cmd.
 
##### Runtime requirements:

 - Python 2.7/3.5+
 

## Building For Dev:

If you're not familiar with make, you should [get familiar with it first](https://www.gnu.org/software/make/#content).

To build a local dev copy in order to do work, you have to build a folder path first. That folder path is:

```markdown
"%APPDATA%\\Adobe\\CEP\\extensions\\protio"
```

or 

```markdown
/Library/Application Support/Adobe/CEP/extensions/protio
```


```markdown
cd /c/dev/protio
yarn global add gulp (This is only required the very first time)
yarn global add jspm (This is only required the very first time)
make deps
make build-dev
gulp watch (Doesn't seem to be picking up on Windows inside build-dev, need to look into this).
```

If you notice inside of the protio source folder, there is a .debug file. This file states what port the extension
is debugged on, which is 6145. Go to `localhost:6145` in a Chrome browser and you can do debugging.

However, you can't do that just yet if this is your first time developing an Adobe CEP extension. You can't even open the
extension inside of Premiere yet, even though it shows up under Window > Extensions > Protio

If you are in the midst of development, you can bypass the check for extension signature by editing the CSXS preference
properties file, located at:

```
Win: regedit >
     HKEY_CURRENT_USER/Software/Adobe/CSXS.8
     Add a new entry PlayerDebugMode of type "string" with the value of "1".
```

```
Mac: In the terminal, type: 
     defaults write com.adobe.CSXS.8 PlayerDebugMode 1
     (The plist is also located at /Users/USERNAME/Library/Preferences/com.adobe.CSXS.6.plist)
```

This puts your Adobe apps into debug mode, and now you can open the application.

While Gulp is running, you can update files and close and reopen the extension to test changes. At a studio that I used
to work at, we had a Ctrl + R refresh working at some point, but an update eventually fucked up what allowed that to work.

Additionally, you can now go to [localhost:6145](http://localhost:6145) and run the debugger.

## Building for Release:


Usage
======
After installing, go to Window > Extensions > Protio

Then, click either the `import otio as sequence` button or the `export sequence as otio` button.


Deployment
===========
If you work in a studio environment... have fun. Adobe deployment for their packages is atrocious
and is not built for a scalable, distributed solution like one that is needed at visual effects studios. 
Ironic, right? They have this desire to want to put everything in their store, but stuff like that does not
work when you're off network, or need to push out constant updates.

At the studio that I used to work at, we had a deployment manager called Ansible (but you can do the same thing
with a Docker container) that moved packages around on disk and we would put a variation of the development 
workflow on disk locally for the editors. 

To build everything out into a deployable package, run 

```markdown
make build
```

and take the results of that build in `./dist/`

It would put an unsigned extension ()in the locations specified above:

```markdown
"%APPDATA%\\Adobe\\CEP\\extensions\\protio"
``` 

and 

```markdown
/Library/Application Support/Adobe/CEP/extensions/protio
```

while doing the debug registry edit/com plist editing - this allowed us to have CI/CD without any trouble.


