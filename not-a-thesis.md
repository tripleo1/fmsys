# UN*X NEEDS A COMPONENT ARCHITECTURE

`fmsys` provides 3 capabilities:

1) Easy packaging/installation
2) Easy end user testing
3) Provide a component model

## Capabilities

### Easy packaging/installation

Traditionally done by an admin, whose job we usurp.
Unitl you of course notice the need for Mandatory Access Control, 
which is specified in the user profile by 

### Easy end user testing

Fiddle around with the sw as soon as it is on the system.
if it installs it works. Most packages will include a demo-space 
folder. This folder contains icons, which when clicked will 
cause the system to create a ViewPart fitting for the package.

They cannot be modified-it is click and run.

### Provide a component model

A component model allows a programmer to leverage other programmers' 
code.  You can use Python components in Java.  You can pull logic 
from one program and almost immediately use it in another.  Without
setting up compilers and cross-compilers and bootstraps.

* `XAD` files.

* Exact dependency matching and automatic download:

  * You can install any number of versions of a component. You should 
    never have to uninstall an old version to install a new version.
    
* Layers:
    * Core: ...
    * Support: Windoing system
    * ???: ...
    
* Windowing package:
    - How many are there? Especially for SDL? They can be consolidated 
    with no loss of functionality, and an actual increase in productivity.
    - Creating demos is easy: just find something similar and code around it!
    
*

* Transport channels

  * __
    - mmap
    - fsaccess
    - socket
    
  * Core access: get basic access to bootstrap ourselves.
	(to create event access.
  * Event access: (`/fm/conf/default-evtaccess`)
    
  * The only user that ca write in fmsys is the fmsys user

*******

```java
    import org.fmsys.fmsys.*;
    
    var o = fmsystem(FSAccess("/fn")); // opr env access
    var comm = o.fmcmpt("/fmdemo/hello-in-python")
    var resp = comm.call("hello.hello", {}); // this call as well can fail
    if resp.code == OK {
	  print resp.value
    } else { 
      print resp.code 
    }
```

`/fm/appz/bin`, `libs`, `runtime`
    
* `runtime`: files used only by this program at runtime
* `bin`: xad files
* `libs`: a personal /usr/lib/*.so
    
`/fm/cmpt/fmdemo/hello-in-python/hello-in-python.py`

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
    
`/fm/data/demo/space/crystal-space/1.demo`
* Load a `xad` file which references a script and/or a layout ...
    
`/fm/data/webserver/htdocs`
* How does a user serve this? (/fm is unmodifiable)
* Same as /var/www; servers always push this shit out. 
* Prolly will have to config (/u/http/home/config/webserver/(apache...))
* `fmsys` wants as little config as possible.
* a prog has only to be compiled once for the version of fmsys its using
	
`/fm/data/docs/<pkgname>/html`...,`README`...
* Avail for docviewer (list on left hand side, viewer on right hand side)

    
`/fm/data/src-ctl/cvs/<repo1>`
`/fm/data/src-ctl/svn/<repo1>`
* `cvsweb` will not need configging (disbale form their interface)

* Once we control our filesystem access, to provide virtual dirs.

`/fm/dvlp/headers` ....

`/fm/tree`
* For migrating programs that dont really belong
    
Resources live in one of 4 storage spaces (not directories):

* `ENTP` -- whatever was made accessible to the network
* `HOST` -- the local workstation/terminal (possibly a PDA or cell phone)
* `USER` -- the user's romaing profile/filesystem
* `VIRT` -- never persisted to disk
    
        
Installed:

`/home/user/fm/cache` --
- `installed` -- active
- `archive` -- disabled
	 
	
`/fm/cmpt/fmdemo/hello-in-python.compdef`
------------------------------------------

```xml
<cmpdef version="1.0">
 <interp engine="cmpt://ns/python/classic" startfile="hello-cmpt.py">
 <process-model>
   <inproc/>
   <exproc>
     <preferred domain="instantiated-from-java"/>
   </exproc>
 </process-model>
 <interface>
   <method name="hello" return="[]" params="[]" />
 </interface>
 <implements cmpt="cmpt://fmdemo/hello-in-py/1.0" />
</cmpdef>
```

`hello-in-java.compdef`
------------------------

```xml
<interp engine="cmpt://ns/java/sun-java" startfile="classname..." />
```

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

