[Back to Tutorial Index](./README.md)

# Tutorial: Personal Blog App

## 1. Getting Started
To start with using Nawah you need to have:
1. [Python](https://www.python.org/) 3.8
2. [MongoDB](https://www.mongodb.com/) 4.2+

You can install the previous prerequisites using your operating system package manage or from their website.

### Using `code-server`
> _Continue if you prefer using `docker`._

If you, for any reason, can't install both or either of the components, or you prefer to keep your computer clean without installing additional components then you can benefit from running a custom [`code-server`](https://github.com/codercom/code-server) that can be used to develop Nawah apps by running [`docker-composer`](https://docs.docker.com/compose) tailor-made for this purpose.

To begin:
1. clone [https://github.com/nawah-io/nawah-code-server](https://github.com/nawah-io/nawah-code-server),
2. edit `PASSWORD` value in `docker-compose.yml` file, and;
3. then run `docker-compose up -d` from the clone directory.

Then, navigate to `https://localhost:8080` in your browser. Enter the password set in your `docker-compose.yml` file. You will be greeted with "Code - OSS" welcome screen. Now, you can follow the tutorial by opening up terminal using `Ctrl+Back-Tick`.

### Installing Nawah CLI

Nawah CLI is the tool to create new Nawah apps. To install Nawah CLI, run*:
```bash
python[3[.8]] -m pip install nawah_cli
```

### Creating New Project
To create new project, open up terminal and run*:
```bash
nawah create my_awesome_blog [/path/to/project] --default-config
```
The argument `/path/to/project` is optional. If you skip it, it will use the current working directory. While argument `--default-config` is passed to create a new app with default config. In future, once you are more familiar with Nawah [App Config Attrs](/reference/app.md), you can create a new app without passing this argument which allows you to customise the app config at time of creation.

### Exploring App Project
The new app project folder has the following structure:
```
my_awesome_blog
├── docker-compose.yml
├── Dockerfile
├── .gitignore
├── LICENSE
├── logs
├── nawah
├── nawah-0.0.whl
├── nawah_app.py
├── packages
│   └── my_awesome_blog
│       ├── __init__.py
│       ├── __l10n__.py
│       └── __tests__.py
├── README.md
├── refs
├── requirements.txt
└── tests
```
The above directories and files are all required for Nawah app to function, however you will not interact with all of them. Following is the description of each:
* **`docker-compose.yml`**, `Dockerfile`: These are the files that you can use to easily deploy your Nawah app. The files provide boilerplate for basic Nawah app. You can modify them according to your needs. `docker-compose.yml` file has an optional `mongodb` service that allows you to start using and testing Nawah apps even if you don't have MongoDB installed on your computer.
* **`LICENSE`**: Empty file to specify your app license, if any.
* **`README.md`**: Boilerplate file to specify your app README, if any.
* **`nawah_app.py`**: App config as Nawah `APP_CONFIG` object. This is where you set overall app Config Attrs, update your app version, and keep track of setting different environments configs.
* **`packages`**: Folder containing all packages required for the app to function.
* **`packages/my_awesome_blog`**: Your app default package. This is where you can start building your Nawah modules for your use.
* **`packages/my_awesome_blog/__init__.py`**: Your default package config as Nawah `PACKAGE_CONFIG`. In single-package apps this has no real value, but it does when you start building multi-package apps. For now you can just consider this file as version reference for your package.
* **`packages/my_awesome_blog/__l10n__.py`**: Your locale terms dictionaries file. Here you can keep reference to those terms for use in your package modules.
* **`packages/my_awesome_blog/__tests__.py`**: Your package tests file. You can add unlimited number of Nawah `TEST` objects representing unit tests or integration tests based on your method of writing the tests.

Other miscellaneous directories and files:
* **`nawah`**: Stubs files for `mypy` linter.
* **`nawah-0.0.whl`**: Wheel file containing Nawah framework, with the API Level present on the file name for easy identification.
* **`.gitignore`**: Basic `gitignore` file based on Github boilerplate.
* **`logs`**: Folder containing logs generated by Nawah framework CLI if `--log` was passed to `launch` command.
* **`refs`**: API references generated with Nawah framework CLI `generate_ref` command.
* **`tests`**: Tests logs generated by Nawah Test Controller and Test Workflow.
* **`requirements.txt`**: Nawah framework requirements. This file is used for installing requirements when app is created, as well as for generating docker images.

With the previous in mind, in basic scenario you will be:
1. Creating new Nawah modules in your default package.
2. Updating your default package `__init__.py` to reflect on required Config Attrs for the package to function.
3. Updating your default package `__init__.py` version.
4. Updating your `nawah_app.py` to reflect on overall app Config Attrs.
5. Updating your `nawah_app.py` version.

The previous scenario might differ based on your use case--Although, Nawah has set of best practices for all aspects of it, you still have the total freedom to follow your own methods as long as you are aware not following the established best practices would mean not being able to upgrade your app to newer versions of Nawah framework, and being left behind with security updates and critical bugfixes.

### Launching App
Your new app is still very basic. It sports all the amazing, built-in Nawah features. Because of this, we can already launch the app and get to confirm it's able to connect to `MongoDB` instance, as well as it's reachable on the same computer.

To being, follow either of these steps based on the applicable condition:
* **Your MongoDB instance is accessible on `localhost:27017` with no username or password (or, you just installed MongoDB on your computer without editing the instance configs)**:
  * Your environnement will be `dev_local`. Keep this in mind and skip to next step.
* **Your MongoDB instance is installed on your computer but requires one or more of the following: username, password, port, or you are using remote instance**:
  * Prepare your MongoDB connection string as following (`HOST_NAME` will be `localhost` if you have MongoDB installed on your computer): `mongodb://[USERNAME:PASSWORD@]HOST_NAME[:PORT][/AUTH_DB]`.
  * Confirm you can access your MongoDB instance using the connection string using `mongo` shell.
  * Open `nawah_app.py` file in your editor of choice.
  * Change the value `mongodb://localhost` on line #11 of the file to the connection string. 
  * Save your changes.
  * Your environnement will be `dev_local`. Keep this in mind and skip to next step.
* **You are using `code-server` image or you prefer to begin with `docker-compose`**:
  * Your environnement will be `dev_server`. Keep this in mind and skip to next step.

Now that we have confirmed app configs are ready for connecting to your MongoDB instance, and we know the environnement we will use, we can attempt to launch our app--Open up terminal and change your directory to app directory, then run (replacing `ENV_NAME` with the environnement name you picked from the previous step):
```bash
nawah launch --env ENV_NAME # REPLACE ENV_NAME!!
```

Now, Nawah CLI would detect you are running it from Nawah app context, so it will switch to Nawah framework CLI (which is byte-coded in wheel file) and pass the call to it. Nawah framework CLI would understand you want to launch the app with the selected environnement, so it will attempt to process collectively:
1. CLI Args passed to Nawah framework CLI (via Nawah CLI),
2. app configs (from `nawah_app.py`),
3. packages configs (from all packages `__init__.py` files), and;
4. some additional meta configs at runtime.

If no surprises are there, your terminal should have a lot of messages showing the process of configuring the app for launch and then tailed with:
```
[INFO]  Welcome to Nawah
======== Running on http://0.0.0.0:8081 ========
(Press CTRL+C to quit)
```

To confirm we are having the app served and ready for connections we can:
* Use your browser and browse: `http://localhost:8081`.
* Use `curl` from your terminal by running: `curl http://localhost:8081`.

Either way, you should be greeted with:
```
{"status": 200, "msg": "Welcome to my_awesome_blog!", "args": {"version": "0.1.0", "powered_by": "Nawah"}}
```

This message serves as confirmation your app is up and running and waiting for connections and requests. It was introduced to [LIMP](/what-is-LIMP.md) to serve a specific role in CI/CD pipelines to assure the expected version of the app is correctly deployed to the deployment target.

#### Troubleshooting Launching App
If you weren't able to launch the app, you probably have one of the following issues:
1. There is always a good hint in Nawah messages. Read the last messages and try to understand what the issue might be.
2. Are you sure you are using the correct environnement to connect to MongoDB instance? Check again first step of this section.
3. Is the terminal showing `Welcome to Nawah` but you can't connect to Nawah app from browser nor `curl`? There can be a possibility `localhost` hostname is not accessible and you need to attempt using something else like `127.0.0.1` or your computer name. There also is the possibility the network service on your computer is blocking access to port `8081` for some reason. You can attempt to change the port by passing `--port [PORT]` to Nawah framework CLI.
4. Are you using `code-server` image? There is the possibility docker was unable to bind the port correctly or that a conflict between docker and the computer network service is making port `8081` unreachable. Try to restart your docker first, then your computer if the first doesn't resolve the issue.
5. Is this not the first time you try running the app? Have you had issues creating the app, so you recreated it again and tried to launch it? There is a possibility your MongoDB instance is limiting your access to the database defined as `data_name` in `nawah_app.py` on line #32. Try changing the name to any other MongoDB-compatible database name.

## Next
Now that you have your first Nawah app ready and proof-tested to launch, you need to start writing modules that allow you to add functionalities to the app. Next, you will learn about creating a blog module that represents the controller and model for a blog post in the app.

[Continue to: 2. Writing Blog Module](./writing_blog_module.md)


\* Square brackets in commands refer to optional or variable parts of the command. For instance, you might need to use `python`, `python3`, or `python3.8` to launch `Python 3.8` on your computer.