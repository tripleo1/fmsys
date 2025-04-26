# UN*X NEEDS A COMPONENT ARCHITECTURE

`fmsys` provides 3 capabilities:

1) Easy packaging/installation
2) Easy end user testing
3) Provide a component model

## Capabilities

### Easy packaging/installation

Essentially formalizing/adapting the drag this (ie dmg) file from the CD to
your /Applications or /System/Applications folder (*cough* directory).

(aside: It would have been so nice if debian (lowercase intentional) did this
way back when, notwithstanding that you needed a 4-year to understand the rest...)

Traditionally done by an admin -- aka, not only do/did you needed sudo (su) for 
this, but you didn't/don't actually *need* it.  You could always get your own 
toolchain up and go from there.

### Easy end user testing

1. Fiddle around with the sw as soon as it is on the system.

2. If it installs it works. -- this is unrealistic, but you know what I mean. 

3. Most packages will include a demo-space folder. 

### Demo space

More or less looks like (clojure) Clerk??

This folder contains icons, which when clicked will
cause the system to create a ViewPart (ie Kparts, all the things that "don't work",
but that actually did.)

They cannot be modified-it is click and run.

### Provide a component model

> This is the nixos part.
> Not extremely different, except for the gRPC/extreme interoperability part.
> The fake json layer (see ice for somebody else's "failure".)

A component model allows a programmer to leverage other programmers' 
code.  You can use Python components in Java.  You can pull logic 
from one program and almost immediately use it in another.  Without
setting up compilers and cross-compilers and bootstraps.

> I smell osquery.

* `XAD` files.

> ~/.local/share/.applications

* Exact dependency matching and automatic download:

  * You can install any number of versions of a component. You should 
    never have to uninstall an old version to install a new version.

> apt/dnf whatever abstracted (how is it not the same thing?)

* Layers:
    * Core: ...
    * Support: Windowing system
    * ???: ...
    
* Windowing package:
    - How many are there? Especially for SDL? They can be consolidated 
    with no loss of functionality, and an actual increase in productivity.
    - Creating demos is easy: just find something similar and code around it!

> gh forks, anyone?

*

* Transport channels

  * __
    - mmap
    - filesystem (they invented tmpfs just for you)
    - socket
    - registry, too if you are that kind of sick and have no group policy

  * Core access: get basic access to bootstrap ourselves.
	(to create event access).
  * Event access: (`/fm/conf/default-evtaccess`)

> I wonder how `conf` works??

  * The only user that can write in fmsys is the fmsys user

> 1 admin user
> 1 user user
> Nobody caught this (A lot of ppl don't care, but for *us*, nobody caught this?)

*******

```java
import org.fmsys.core.*;

var o = fmsystem(FSAccess("~/.config/fmsys")); // opr env access
var comm = o.fmcmpt("/fmdemo/hello-in-python");
var resp = comm.call("hello.hello", {}); // this call as well can fail
if resp.code == OK {
  print resp.value
} else { 
  print resp.code 
}
```

> Leave it alone, before you go crazy.
> Respectfully, /fm (and a whole bunch of other ones, ie the home dir) are
> a mess of unions and bind mounts
> . 
> Everything is "bubblewrapped" and under it's own user
> (aside Linux was not as friendly to this in the past as it is now.)

#### Looks like for each app

`/fm/appz/<rdns>/bin`, `libs`, `runtime`
    
* `runtime`: files used only by this program at runtime
* `bin`: xad files
* `libs`: a personal /usr/lib/*.so

> Most dynamic linking was replaced by (extremely fast, possibly dynamic) rpc
> once again: not actually (traditional) unix.

`/fm/cmpt/fmdemo/hello-in-python/hello-in-python.py`

> This is not rdns...

* ...

`/fm/libs`

* Something that everyone would use and not really a component
* We wish to replace everything down to the C and C++ libs
* Components are dynamically loaded, libs are never dyn loade
    
`/fm/syst/conf/compcache`
* ??

`/fm/cnfg`
* Admin's policy

`/fm/data/controlpanel/bin/gnome.xad`
* Maybe this goes in `demo`?

> Interesting.
 
`/fm/data/demo/space/crystal-space/1.demo`
* Load a `xad` file which references a script and/or a layout ...

> These are shipped.
> Derive them at your pleasure/leisure.
> A clear button like in gnome40+ is provided.
> The details are of course an individual thing, but the framework
> (like how github provides you with a lot (lab is better, sometimes),
> depending on what you want to do.)

`/fm/data/webserver/htdocs`
* How does a user serve this? (/fm is unmodifiable)
* Same as /var/www; servers always push this shit out. 
* Prolly will have to config (/u/http/home/config/webserver/(apache...))
* `fmsys` wants as little config as possible.
* a prog has only to be compiled once for the version of fmsys its using

`/fm/data/docs/<pkgname>/html`...,`README`...
* Avail for doc viewer (list on left hand side, viewer on right hand side)

`/fm/data/src-ctl/cvs/<repo1>`
`/fm/data/src-ctl/svn/<repo1>`
* `cvsweb` will not need configging (disable from their interface)

> Used to be more irritating.  Not that big of a deal.  Still irritating, tho.

* Once we control our filesystem access, to provide virtual dirs.

`/fm/dvlp/headers` ....

> You see it.

`/fm/tree`
* For migrating programs that don't really belong

> Being lazy b/c we were on Linux and didn't want to keep "frame switching".

### Resources live in one of 4 storage spaces (not directories):

> The closest thing I have seen is in some of the cloud stuff.

* `ENTP` -- whatever was made accessible to the network
* `HOST` -- the local workstation/terminal (possibly a PDA or cell phone)
* . 
* `USER` -- the user's roaming profile/filesystem

> Playing with that system.
> Local network was always questionably slow. Never knew why.

* `VIRT` -- never persisted to disk

> Script kiddies, put viruses here.

Installed:

`/home/user/fm/cache` --
- `installed` -- active
- `archive` -- disabled

> aside: go, groovy/gradle, clojure, pip, npm, ad nauseam in ~/.cache

`/fm/cmpt/fmdemo/hello-in-python.compdef`
------------------------------------------

```xml
<?xml version="1.0" encoding="US-ASCII" ?>
<compdef version="1.0">
 <interp engine="cmpt://ns/python/classic" startfile="hello-cmpt.py"/>
 <process-model>
   <!-- explicitly allow embedding -->
   <inproc/>
   <!-- explicitly fork when we have a conflict, esp of native libs. -->
   <exproc>
     <preferred domain="instantiated-from-java"/>
   </exproc>
 </process-model>
 <interface>
   <method name="hello" return="[]" params="[]" />
 </interface>
 <implements cmpt="cmpt://fmdemo/hello-in-py/1.0" />
</compdef>
```

> Highly biased. (I cannot read Devangari and you know that.)
> IDEA editing fragments is superb and desired. (Good job)


`hello-in-java.compdef`
------------------------

```xml
<interp engine="cmpt://ns/java/sun-java" startfile="classname..." />
```

> clj -M -m classname ...

* Must actually be a component
* `ns/java/interp`


`hello-in-python.py`
------------------------

```python
from fmsys import *
import hello
 
def start(syst, paramdict):
    syst.expose("hello", helo.hello)
def handle_req(from_ch, t_ch):
    ....
```

> idk if this is wrong or not
