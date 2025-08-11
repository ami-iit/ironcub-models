# ironcub-models
This repo contains the `urdf` and `xml` models of iRonCub.

# Installation

To install the repo run:

```sh
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=<install-prefix> ..
make
(make install)
```

In order to use the model, the following env variables must be configured:
```sh
# iRonCub model in YARP
export YARP_DATA_DIRS=${YARP_DATA_DIRS}:<install-prefix>/share/iRonCub-Mk3/iRonCub/robots
# iRonCub model in Gazebo
export GAZEBO_MODEL_PATH=${GAZEBO_MODEL_PATH}:<install-prefix>/share/iRonCub-Mk3/iRonCub/robots
export GAZEBO_MODEL_PATH=${GAZEBO_MODEL_PATH}:<install-prefix>/share
```
