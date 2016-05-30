# Codurance site with Hugo


## What do you have to install? 

Nothing

## What do you have to do to run the site locally?

**No languages, No dependency managers** 
 
**Just run:**

#### on Mac OS/X:

    ./bin/hugo server --buildDrafts --verbose --verboseLog --watch

#### on Windows:

    ./bin/hugo_windows_amd64.exe server --buildDrafts --verbose --verboseLog --watch

**If your Windows OS is 32-bit version you can use *hugo_windows_386.exe* instead of *hugo_windows_amd64.exe***

#### on Linux:

    ./bin/hugo_linux_amd64 server --buildDrafts --verbose --verboseLog --watch

**and enjoy our website in a web browser:**
 
    http://localhost:1313/

## How to edit the site?

Change or add elements to the `content` directory and follow start the Hugo server (see above).

The structure of the `content` directory describes also the structure of our website.

If you want to know how does it work read [HUGO.md](HUGO.md).