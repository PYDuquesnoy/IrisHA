# IRIS High Availability and Scalability



This is a set of docker-compose files to bring up servers that allow to build manually some IRIS Cluster configurations

* MirrorDemo

  2 IRIS Servers (irisM1 and irisM2) and one arbiter (arbiter) that can be configured as a Failover Mirror. 

  

* iris-mirror-with-docker

  a clone of karetdev's repository with scripts that automatically configure the 2 mirror members and setup mirroring of an application database. All the manual steps of MirrorDemo get automatically performed on startup

  

* AppServerDemo

  2 ECP Application Servers and 1 Data Master Server. Allows to configure ECP manually to share a remote database USER.



## MirrorDemo

To run this demo, install an valid IRIS license "iris.key" in the license subdirectory. The license needs to be for "container" platform, and have multi-server enabled (for mirroring).

Note:  the docker-compose deifines an external network "sam-100115-unix-default", as following:

```
networks:
    default:
      external:
        name: sam-100115-unix_default
```

This network gets started in the separate SAMDemo, and allows both demo to share the same network, so that SAM can be used to monitor the Mirroring cluster. If you don't have SAMDemo running, you can simply delete the network definicion from the docker-compose file.



When ready, run the docker-compose:

```
docker-compose up
```

Connect with a browser to each server

| Server    | URL from Host                               |
| --------- | ------------------------------------------- |
| webserver | http://127.0.0.1:20080/csp/sys/UtilHome.csp |
| irisM1    | http://127.0.0.1:22773/csp/sys/UtilHome.csp |
| irisM2    | http://127.0.0.1:32773/csp/sys/UtilHome.csp |



From within the docker network, the servers can reference each other by names "irisM1", "irisM2", "arbiter", and use the ports 2188 for ISCAgent and 51773 for SuperServer in their mirroring configuration.

The docker-compose already starts up the ISCAgent on each iris node, so you can directly enable and configure mirroring in the portal after logging in.

Each iris instance has a /logs directory shared with the host. This can be used if you setup strutured logging.





## iris-mirror-with-docker

This demo performs all the previous mirror configuration steps on startup.

To run the demo, provide a valid "iris.key" in the main directory and start with:

```
docker-compose up 
```

Then go to the portal to review the mirrored configuration.

## AppServerDemo

Similar to MirrorDemo:

* add a valid iris.key in the ./license directory

* Review the networks definition of the docker-compose file and adjust (delete if SAMDemo is not started)

* Start the configuration with

* ```
  docker-compose up
  ```

Connect with a browser to each server, and Setup ECP between the App Servers and the irisM1 DB instance:

| Server | URL from Host                               |
| ------ | ------------------------------------------- |
| irisM1 | http://127.0.0.1:32000/csp/sys/UtilHome.csp |
| app1   | http://127.0.0.1:12773/csp/sys/UtilHome.csp |
| app    | http://127.0.0.1:22773/csp/sys/UtilHome.csp |