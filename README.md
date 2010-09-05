Tiny Bash Server
=====================

Author: Sayan "Riju" Chakrabarti  
Contact: me[at]sayanriju.co.cc  
Software License: GNU GPL v3


About
--------
Tiny Bash Server is a small http server coded entirely using Bash and friends. It allows CGI style scripting
with .htsh files which may contain Bash code embedded within normal HTML.

TBS uses netcat to bind itself to open port(s). Multiple instances of the server may be run 
(on different ports and with different docroots) using separate configuration files.

TBS comes with all the basic features you expect of a web server : serving HTML/CSS, handling POST/GET forms, etc.
It also passes selected "Environmental Variables" for use with CGI scripting in .htsh files.  

However, it is highly **NOT** recommended to run TBS on any sort of production system. This is because, as a server,
TBS is relatively slow, potentially insecured and has less features than full-fledged servers like, say Apache.    
After all, this project is rather a hack, and was coded just for fun and some sort of a PoC.

A nice use for TBS might be to develop browser-based frontends to bash scripts for local usage, in which
scenarios it carries lesser overhead than full-blown server setups.


Requirements
--------------------

Following are the requirements for running TBS which are nominal, and are expected to be installed by default
 in most Linux distributions. Figures within brackets indicate versions on which it was tested.

* Bash (4.0.33)
* Netcat (1.10-38)
* GNU grep (2.5.4)
* GNU sed (4.2.1)
* GNU coreutils (7.4)


**Note:** Netcat must be compiled with option `-DGAPING_SECURITY_HOLE` enabled. Also, a working /bin/sh is required (Note that POSIX-conformant system must have 
one)


Installation & Usage
---------------------

After downloading, the contained files are to be placed under / maintaining the provided hierarchy. All files in usr/bin must have
executable permissions.  
Right now, the default installation options are rather hard coded.

####To start TBS:


>`/usr/bin/tbs.start  [/path/to/config/file]`

This will start TBS based on options specifed in the configuration file (optional argument). Generally, you don't need to have root privileges to start/stop TBS.

If no argument be passed, it uses the following default configuration file located at /etc/tbs/default.config   
whose syntax may be used as reference:

>`DOCROOT=/var/www/`  
>`PORT=1983`  
>`DEFAULT_INDEX=index.html`  
>`QUIET=0`

####To stop TBS:

>`/usr/bin/tbs.stop  [port]`

This will stop any instance of TBS running on the specified port. If no port be specified,
it will try to stop TBS on port 1983 (default).

Note that, you may need to pass one more request to the server after issuing the above command, before TBS actually stops.
If the instance is running with option QUIET=0, a simple CTRL+C on the terminal where tbs.start is running would suffice!



CGI Style Scripting with .htsh files
------------------------------------

TBS allows CGI style scritping with .htsh files (which contains normal HTML with bash code embedded within) by
means of an included htsh parser.  
All bash codes are to be enclosed within `<?bash ... ?>` or in short, `<? ... ?>` tags. 

The following "Environment Variables" are passed by the server to the CGI for usage within the .htsh:

>`DOCUMENT_ROOT`   
>`HTTP_HOST`  
>`HTTP_USER_AGENT`  
>`QUERY_STRING`  
>`REQUEST_METHOD`  
>`REQUEST_URI` 

All of the above carry their usual meanings, except QUERY_STRING, which is a bit different for TBS.
In most other servers, this variable is valid only in case of GET requests and refers to the part of the URL after the ?  
In TBS though, this variable is valid for POST requests as well, and has the same format as for GET (e.g myvar1=0&myvar2=1)

All these are available as normal bash variables within .htsh scripts.  



