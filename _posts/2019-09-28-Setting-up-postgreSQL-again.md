
dlopen(/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages/psycopg2/_psycopg.cpython-37m-darwin.so, 2): Library not loaded: libssl.1.1.dylib
  Referenced from: /Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages/psycopg2/_psycopg.cpython-37m-darwin.so
  Reason: image not found
  
  https://stackoverflow.com/questions/16407995/psycopg2-image-not-found
  
  sudo ln -s /Library/PostgreSQL/9.2/lib/libssl.1.0.0.dylib /usr/lib
$ sudo ln -s /Library/PostgreSQL/9.2/lib/libcrypto.1.0.0.dylib /usr/lib



sudo ln -s /Applications/Postgres.app/Contents/MacOS/lib/libssl.1.0.0.dylib /usr/lib 
sudo ln -s /Applications/Postgres.app/Contents/MacOS/lib/libcrypto.1.0.0.dylib /usr/lib


... but there's hardly anything in the macos directory here!  

Uninstalled postgres.app: https://postgresapp.com/documentation/install.html

Going to try just installing correct postgresql version and connecting to pgadmin4...


Uninstalled and reinsalled psycopg2, got this error again:
Error: pg_config executable not found.
    
    pg_config is required to build psycopg2 from source.  Please add the directory
    containing pg_config to the $PATH or specify the full executable path with the
    option:


export PATH=$PATH:/library/postgresql/10/bin
(only updates it for the current session)



Now able to install psycopg2 (2.8.3), unable to import:

/library/postgresql/10/lib
libcrypto.1.1.dylib
libssl.1.1.dylib


  File "/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages/psycopg2/__init__.py", line 50, in <module>
    from psycopg2._psycopg import (                     # noqa
ImportError: dlopen(/Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages/psycopg2/_psycopg.cpython-37m-darwin.so, 2): Library not loaded: libssl.1.1.dylib
  Referenced from: /Library/Frameworks/Python.framework/Versions/3.7/lib/python3.7/site-packages/psycopg2/_psycopg.cpython-37m-darwin.so
  Reason: image not found
 
 This might work:
https://stackoverflow.com/questions/16407995/psycopg2-image-not-found

cd /
  sudo ln -s /Library/PostgreSQL/10/lib/libssl.1.1.dylib /usr/lib
  sudo ln -s /Library/PostgreSQL/10/lib/libcrypto.1.1.dylib /usr/lib

... But this produced an identical error!


https://github.com/psycopg/psycopg2/issues/385


Try just installing conda (that is, miniconda - conda and its dependencies)

Still no go


... noticed that the path mentioned in the error was still the one used by pip, rather than by conda.
Removed these files (even though I had run pip uninstall)
Now module not found... trying to add to path variable in macos turns out to be difficult.

https://scriptingosx.com/2017/04/on-bash-environment-variables/

PATH=${PATH}:/Users/david/miniconda3/lib/python3.7/site-packages
export PATH


Try creating a conda environment, installing, then activating and trying to import:
https://ncgas.org/Blog_Posts/Installing%20conda%20packages%20locally.php


Activated the conda environment, installed pscyopg2, switched to this interpreter in pycharm, ran original script that used psycopg2... and it worked!

Misc other:
https://jakevdp.github.io/blog/2017/12/05/installing-python-packages-from-jupyter/
https://stackoverflow.com/questions/19085807/please-enter-a-commit-message-to-explain-why-this-merge-is-necessary-especially


