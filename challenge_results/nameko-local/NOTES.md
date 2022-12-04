# Deploying nameko-devex local envinroment
![Airship Ltd](airship.png)


## Important Notes
* Deploy nameko microservice in local machine

1 -  Because the MacOS Ventura version I needed to install an developer package called xcode. 
This solution solve a xcrun error when executed a first application build and git commands.

### References

* Issue Solutions:
    - [Developer Apple](https://developer.apple.com/forums/thread/673827)
    - [Stackexchange Apple](https://apple.stackexchange.com/questions/254380/why-am-i-getting-an-invalid-active-developer-path-when-attempting-to-use-git-a)
    - [Cyberithub] (https://www.cyberithub.com/solved-xcrun-error-invalid-active-developer-path-library-develop/)


2 - I install manualy the alembic packege with "pip install alembic" to excecute run.sh script into the dev_run.sh script.
Alembic as I understand, is a database handle tool. And in this case, it's necessary to execute a specific query into de database from application.

### References

* Issue Solutions:
    - [Stackoverflow](https://stackoverflow.com/questions/30578660/alembic-distribution-was-not-found-error)
    - [Alembic](https://alembic.sqlalchemy.org/en/latest/)


3 - I install manualy a lot of modules and librarys like debugpy, psycopg2-binary, gevent, marshmallow, nameko_sqlalchemy, bzt and redis to solve a lot of application errors during ./dev_run.sh orders.service products.service script execution.

```sh
ModuleNotFoundError: No module named 'debugpy'

ModuleNotFoundError: No module named 'psycopg2'

ModuleNotFoundError: No module named 'gevent'

ValueError: No module named 'marshmallow'

ValueError: No module named 'nameko_sqlalchemy'
```

### References
- [Copypaste](https://copypaste.guru/WhereIsMyPythonModule/how-to-fix-modulenotfounderror-no-module-named-debugpy)


4 - EntryPoints Error

```sh
'EntryPoints' object has no attribute 'get'
```

### Solution
To solve 'EntryPoints' object has no attribute 'get' issue, I needed to do the downgrade from  "importlib-metadata" module. 
Because the version installed was not compatible with the function required.
Was necessary to insert in the Dockerfile of each application microservices the follow command
```sh
$ pip install importlib-metadata==4.13.0 
```

### References

* Issue Solution:
    - [Stackoverflow](https://stackoverflow.com/questions/73929564/entrypoints-object-has-no-attribute-get-digital-ocean)


5 - Error ocurred executing ./test/nex-smoketest.sh local - Probably related with Marshmallow and requests python lib.

```sh
Local Smoke Test
STD_APP_URL=http://localhost:8000
=== Creating a product id: the_odyssey ===
{"error": "UNEXPECTED_ERROR", "message": "__init__() got an unexpected keyword argument 'strict'"}
=== Getting product id: the_odyssey ===
{
  "error": "PRODUCT_NOT_FOUND",
  "message": "Product ID the_odyssey does not exist"
}
=== Creating Order ===
{"error": "UNEXPECTED_ERROR", "message": "__init__() got an unexpected keyword argument 'strict'"}
=== Getting Order ===
parse error: Invalid numeric literal at line 1, column 10
```

### References
- [Github](https://github.com/psf/requests/issues/5959)
- [Stackoverflow](https://stackoverflow.com/questions/70607987/requests-init-got-an-unexpected-keyword-argument-strict)


### Important Final Note

Unfortunately I was not be able to spin-up de nameko-devex in the local machine, that I've been faced to a lot of aplications and libs errors and I wouldn't have enough time to solve them.

After solved a lot of lib and modules versions issue, finally this error ```sh {"error": "UNEXPECTED_ERROR", "message": "__init__() got an unexpected keyword argument 'strict'"}``` appear and I tried solve, but I can't.
