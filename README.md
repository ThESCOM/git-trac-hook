<pre><code>             ,
         -  /|
        -  / |
      -   /o |
   ~^~ ^~//-'|~^~ ~^~
    ~^~.-->>---. ~^ ~^~
   ~^~ `"""""""` ^~^ ~^
    ~^~ ~^ ~^~^ ~^~^ ~^
    Max Oberberger
    github@oberbergers.de
    https://github.com/chiemseesurfer/git-trac-hook
</code></pre>
Copyright &copy; 2012 Max Oberberger

Table of Contents
=================
1. [Introduction]()
2. [System Requirements]()
3. [Installation]()
4. [Using git-trac-hook]()
5. [References]()


1. Introduction
===============
If you are using git [1] for source control and Trac [2] including GitPlugin 
for bug tracking, then the git-jira-hook script might be useful to you.

Once you have the script setup in your environment, every time you 
make a git commit, a comment is automatically posted to an open 
issue in Trac.

2. System Requirements
===============
- Python 2.x and python modules: subprocess, trac-modules
	  I've tested with python 2.6.6

- A Trac installation with Git-Plugin or the newest Trac (1.0)
  Trac got a build-in support for Git with its newest version:
  http://trac.edgewall.org/wiki/TracGit
  http://trac-hacks.org/wiki/GitPlugin

- Git Version 1.6.x.y or newer (I've tested with 1.7.2.5)


3. Installation
===============
Here is a typical example of how this hook may be used.

In a corporate setting where git is used, there is typically an 
"upstream" or "public" repository. And then developers have their 
"private" repositories.  For their day-to-day work, the developers 
use their private repositories.  Periodically, they (either 
directly, or via gatekeepers) push changes from their private 
repository to the "upstream" one. Also, the "upstream" repository 
is typically a bare repository, and no actual commits are done
here.

In the private repository, the hook should be installed, but only 
*partically*. Every time a commit is made, the hook only checks to
make sure that the commit text conforms to correct formatting (i.e. 
the magic references to Trac issues are present). No Trac issues 
are updated.

The hook is completely installed in the "upstream" repository.

Whenever a commit is made in the upstream repository or a "git push" 
is done to it, the  installed hook will kick in and validate the 
commit message, followed by update of the Trac issue.

3.1 Installation for "upstream" repository
------------------------------------------
- Copy this script to
  <upstream-repo-GIT-dir>/hooks/{pre-receive|post-receive}
  and mark it executable

  **Example:**
    
        cp git-trac-hook upstream-project.git/hooks/post-receive
        cp git-trac-hook upstream-project.git/hooks/pre-receive
        chmod a+x upstream-project.git/hooks/post-receive
        chmod a+x upstream-project.git/hooks/pre-receive

- Set the following git config values in the bare repository:
  
  		git config trac.env /path/to/your/trac/environment
  		git config trac.admin /path/to/your/trac-admin
  		git config trac.depth NR

  **Example:**
    	
    	git config trac.env /usr/local/trac/myTrac
    	git config trac.admin /usr/local/bin/trac-admin
    	git config trac.depth 4

	trac.depth is used to extract the name of the git repository.
	
	**Example:**  
		Repo: /var/www/git/repo.git => trac.depth 4

4. Using git-trac-hook
===============
When you are read to make a git commit, make sure that you have an appropriate
open trac issue.

Anywhere in your commit message, you must put the following strings (without the
quotes):
   "refs #23"
   "refs #1"
   "fixes #8"

If you want to commit things not according to a trac-ticket, you can use 
   "#noref"
With this tag, no trac issue update will be done.

And then you do a "git push" to push your changes upstream, the trac issue
update will be done.

A complete list of all tags which are available
   - close
   - closed
   - closes
   - fix
   - fixed
   - fixes
   - references
   - refs
   - addresses
   - re
   - see
   - #noref


5. References
===============
[1] Git, an source configuration management ("SCM") tool
    http://git-scm.com/

[2] Trac, a bug tracking system
    http://trac.edgewall.org

[3] Trac, Git Plugin
    https://github.com/hvr/trac-git-plugin
    http://trac-hacks.org/wiki/GitPlugin
