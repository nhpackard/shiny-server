# Shiny Server

Shiny Server is a server program that makes [Shiny](https://rstudio.com/shiny) applications available over the web.

## V. 1.4.2

This is an historical version for 1.4.2, because 1.5.x has problems on the mac.

## Features

* Host multiple Shiny applications, each with its own URL
* Can be configured to allow any user on the system to create and deploy their own Shiny applications
* Supports non-websocket-capable browsers, like IE8/9
* Free and open source ([AGPLv3](http://www.gnu.org/licenses/agpl-3.0.html) license)
* Pre-built installers for select Linux distributions.
* **Experimental quality. Use at your own risk!**

## Installing

At this time, Shiny Server can be run on Linux servers with explicit support for Ubuntu 12.04 or greater (64 bit) and CentOS/RHEL 5 (64 bit) or greater. If you are using one of these distributions, please download the pre-packaged installers from RStudio:

> [Download Shiny Server Installers](https://www.rstudio.com/products/shiny/shiny-server/). 

These installers will provide a majority of the prerequisite software and will provision all the necessary directories for you.

If you are not using one of the explicitly supported distributions, you can still use Shiny Server by building it from source, see the [instructions for building from source](https://github.com/rstudio/shiny-server/wiki/Building-Shiny-Server-from-Source).

## Installing from source summary

```
# Clone the repository from GitHub and cd to there, e.g.:
git clone https://github.com/nhpackard/shiny-server.git shiny-server-v.1.4.2
cd shiny-server-v.1.4.2

# use the appropriate version of launcher.cc:
# Either linux:
cp src/launcher.cc.org src/launcher.cc
# or mac:
cp src/launcher.cc.mac src/launcher.cc

# Get into a temporary directory in which we'll build the project
mkdir tmp
cd tmp

# Add the bin directory to the path so we can reference node
DIR=`pwd`
PATH=$DIR/../bin:$PATH

# See the "Python" section below if your default python version is not 2.6 or 2.7. 
PYTHON=`which python`

# Check the version of Python. If it's not 2.6.x or 2.7.x, see the Python section below.
$PYTHON --version

# Use cmake to prepare the make step. Modify the "--DCMAKE_INSTALL_PREFIX"
# if you wish the install the software at a different location.
cmake -DCMAKE_INSTALL_PREFIX=/usr/local -DPYTHON="$PYTHON" ../
# Get an error here? Check the "How do I set the cmake Python version?" question below

# Recompile the npm modules included in the project
make
mkdir ../build
(cd .. && ./bin/npm --python="$PYTHON" install)
(cd .. && ./bin/node ./ext/node/lib/node_modules/npm/node_modules/node-gyp/bin/node-gyp.js --python="$PYTHON" rebuild)

# Install the software at the predefined location
sudo make install
```

## Configuration

Shiny Server will use the [default configuration](https://github.com/rstudio/shiny-server/blob/master/config/default.config) unless an alternate configuration is provided at `/etc/shiny-server/shiny-server.conf`. Using the default configuration, Shiny Server will look for Shiny apps in `/srv/shiny-server/` and host them on port 3838. If you plan to host your apps in this directory, you can either copy an app you've already developed to that location:

```
sudo cp -R ~/MY-APP /srv/shiny-server/
```

Or you can copy some or all of the examples provided with the Shiny package. (The location of the R library varies from system to system. You can use the command `R -e ".libPaths()" --quiet` to print the directory of the R library.) For instance, on Ubuntu, you could execute `cp -R /usr/local/lib/R/site-library/shiny/examples/* /srv/shiny-server/`.

Now start a web browser and point it to `http://<hostname>:3838/APP_NAME/`

**If the browser is not able to connect to the server, configure your server's firewall to allow inbound TCP connections on port 3838.**

To customize any of the above, or to explore the other ways Shiny Server can host Shiny apps, see the [Shiny Server Configuration Reference](https://rstudio.github.io/shiny-server/latest/#configuration-settings) for details on the various ways Shiny Server can be configured.

## Documentation & Contact & Support

See [the Administrator's Guide to Shiny Server](https://rstudio.github.io/shiny-server/latest/) for more complete documentation regarding the setup and management of Shiny Server.

Please direct questions to the [shiny-discuss](https://groups.google.com/group/shiny-discuss) mailing list. If you're interested in Professional Support, please look at our commercial Shiny Server Professional product.
