# ConductR setup using Vagrant - Sample

Sample project to illustrate how to setup 3 node ConductR cluster using Vagrant.

## Prerequisite

Ensure [Vagrant](https://www.vagrantup.com) is installed and working.

ConductR is **not** provided by this repository. Visit the [Customer Portal](https://together.typesafe.com/) to download or [Typesafe.com](https://www.typesafe.com/products/conductr) to sign up to evaluate ConductR.

Copy the ConductR deb installation package into the `conductr/debs` folder in your local copy of this repo. The installation package will be referenced by the `Vagrantfile` as part of the setup.

## Setup

Build the base image containing JDK8 and HAProxy.

```
cd base
vagrant up
vagrant package --output conductr-base.box
vagrant add conductr-base conductr-base.box
cd ..
```

Build the 3 node ConductR cluster.

```
cd conductr
vagrant up
```

The 3 node cluster will be available on `192.168.33.10`, `192.168.33.11`, and `192.168.33.12` respectively.

To test that the cluster has been setup successfully, access `http://192.168.33.10:9005/members` to view the ConductR cluster member information. There should be 3 members setup within the cluster, e.g.

```
{
    "members": [
        {
            "node": "akka.tcp://conductr@192.168.33.10:9004",
            "nodeUid": "783272787",
            "roles": [
                "all-conductrs"
            ],
            "status": "Up"
        },
        {
            "node": "akka.tcp://conductr@192.168.33.11:9004",
            "nodeUid": "-516607461",
            "roles": [
                "all-conductrs"
            ],
            "status": "Up"
        },
        {
            "node": "akka.tcp://conductr@192.168.33.12:9004",
            "nodeUid": "-784617439",
            "roles": [
                "all-conductrs"
            ],
            "status": "Up"
        }
    ],
    "selfNode": "akka.tcp://conductr@192.168.33.10:9004",
    "selfNodeUid": "783272787",
    "unreachable": []
}
```

To access the cluster member via SSH, execute:

```
vagrant ssh ${CONTAINER_NAME}
```

where `${CONTAINER_NAME}` is `cond-0`, `cond-1`, and `cond-2` for each member of the cluster respectively.
