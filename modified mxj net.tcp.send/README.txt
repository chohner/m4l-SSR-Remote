Pre-compiled net.tcp.send

This folder contains the files 'send.java' and 'send.class' to work with the M4L SSR Remote, modified to use binary zero ('\0') as the default line ending.

On Mac OS X, right-click Max.app, select 'Show Package Contents' and navigate to the following directory:

Max.app/Contents/Resources/C74/packages/max-mxj/java-classes/classes/net

BACKUP the original 'send.class' and 'send.java' (by renaming or moving to a seperate folder) and drop the provided files there. This is without warranty and has only been tested on OS X Yosemite (10.10.5).

Please be aware that this does change the default behaviour of net.tcp.send. To reverse the line ending to the systems default, simply remove the pre-compiled files and restore the original files. A restart of Max might be required.
