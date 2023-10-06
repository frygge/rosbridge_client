# rosbridge_client

This package provides a client for the rosbridge_server out of the rosbridge_suite.

## Installation

To install the package, first install the dependencies.
```
rosrun bobdep install rosbridge_client
pip install rospy_message_converter
```
Note, that there is one package that is not in the ROS distro, so it has to be installed manually.
Then, install the package itself.
```
catkin build rosbridge_client
```

## Nodes

### rosbridge_transceiver

This node connects to a remote ROS instance running a rosbridge_server via a websocket.
It lets the user chose a set of local and remote topics, which shall be forwarded to the remote and local site, respectively.

#### Parameters

* ``hostname`` is the hostname of the machine running the remote ROS instance. Default: localhost
* ``port`` is the port of the remote rosbridge_server. Default: 9090
* ``send`` is a comma-separated list of local topics to forward. An alternative remote topic name can be provided.
* ``recv`` is a comma-separated list of remote topics to receive. An alternative local topic name can be provided.

Alternative remote (local) topic names when sending (receiving) topics have to follow the local (remote) topic name delimited via a colon ':'.
For example, the following argument for the ``send`` parameter would forward the local topics l1 and l2 to remote topics r1 and r2.
```
_send:=l1:r1,l2:r2
```
The next example receives remote topic `r` and publishes the messages locally via topic `l`.
```
_recv:=r:l
```

#### Example

The system can be tested on a single machine as follows.

Install the rosbridge_suite:
```
apt install ros-noetic-rosbridge-suite
```

Run the server:
```
roslaunch rosbridge_server rosbridge_websocket.launch port:=9999
```
Note, that the port parameter can be omitted.
In this case, the default port `9090` will be used.

After installing the package locally, run the client node:
```
rosrun rosbridge_client rosbridge_transveiver _port:=9090 send:=/in:/out _recv:=/rin:/rout
```

Now, running two `rostopic echo` commands will listen to the forwarded topics.
```
rostopic echo /out
rostopic echo /rout
```

Sending two messages will yield an output on the two topics above.
```
rostopic pub -1 -s /in std_msgs/String "{data: 'Hello from local'}"
rostopic pub -1 -s /rin std_msgs/String "{data: 'Hello from remote'}"
```
