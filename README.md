MADLET Test Plan
=======

## Introduction

The purpose of this document is to explain the objectives, targets, and strategies for validating and testing the installation of the MADLET software.  The testing plan is to be executed at each phase of the MADLET's software operational lifecycle.  This document is will explain the purpose of this testing, a strategy for testing each phase, the conditions under which the testing phase is considered a success, roles under which testers, developers, and user fall under, and risk mitigation strategies.

## Assumptions

The following documents the assumptions under which the stakeholders, which include the testers, developers, and users will be operating under during each testing phase.  Risk mitigation strategies should these assumptions fail will be documented under the risk mitigation section.  Assumptions specific to each testing objective will be described in the testing strategy under each objective.

It is assumed that the tester has physical access to the system in which the MADLET software will be installed.  The tester must have some form of administrative access to the system, such as the ability to sudo to the root user, the ability to transfer software to and from the system, and the ability to reconfigure the system as needed.

## Objectives

Each objective validates a phase of the operational lifecycle of the MADLET software.  Please note that each objective listed here is in order, and that the previous objective must complete pass with a success condition before the next set of testing objectives be performed.

### Environmental Installation Testing

#### Purpose

The purpose of this testing is to validate the environment in which the MADLET software will be installed.  This testing must be done before MADLET software is installed on the system.

#### Testing Strategy

The tester will validate the following:

* the operating system in which the system is installed will be a Red Hat 7 operating system
   * this objective can be verified by running ```cat /etc/redhat-release ```.  The value must be **Red Hat Enterprise Linux Server release 7.6 (MAIPO)**

* the system in which the MADLET software will be installed will have a route to a SUMP server
   * The SUMP server must have a valid hostname and port
   * this objective can be verified by running ```echo hello > /dev/udp/<sump_server>/<sump_port>```.  If successful, the output will be silent.
   
* A service user called madlet must have been created on the system

* the system has access to the AATS and JAWS sensor data through a NAS, or some equivalent data
   * The path must be known and available to the madlet user

* the system has access to the Custody and Tactical Raven sensor data through some form of a network mounted data source
   * The path must be known and available to the madlet user

* the physical system that this software is installed on has NVidia GPU capability
   * This can be tested by running any known nvidia utility that is installed as part of the capability installation, such as ```nvidia-smi```

* the following ports must be available on the system
   * 8501
   * 8091
   * 8092
   * 8093
   * 5672

#### Success Conditions

This test objective must be complete fulfilled.  Each capability must be verified before software installation should begin.  A failure of any one of these sub objectives causes a complete failure of the entire objective.  

### Post Installation Testing

#### Purpose

The purpose of this testing is to validate the software installation after the MADLET software has been installed on the system.  This testing is to be done after the MADLET software artifacts have been installed on the system.

#### Testing Strategy

The tester will validate that all the RPM artifacts associated with the MADLET software was correctly installed.  This can be done by running ```rpmquery -q``` on each of the following installed software packages:
* tensorflow_api
* http_api
* s4_client
* satnet-results-server
* tensorflow_model_server
* madlet-nodejs
* madlet-tensorflow
* infer
* yolo-rotate
* yolo-single

Developers will provide a script called ```madlet-install```.  This script can be run, assuming the script is the current working directory
```
./madlet-install --check_install
```

#### Success Conditions
If the script ```madlet-install``` returns a successful exit code, then the software packages have been successfully installed.  Missing packages that as detected by the ```madlet-install`` script constitute a failure of this testing objective.

### Configuration Testing

#### Purpose

The purpose of this testing is to validate the software was correctly configured after the MADLET software has been correctly installed.  This testing is to be done after the MADLET software has been validated to have been installed on the system. 

#### Testing Strategy

The system administrator and/or the software developers, possible following a form of DevOps strategy, will have access to all information from the first objective.  The person configuring the software will have access to the documentation needed to correctly configure the software.  The tester will also have full access to the documentation that was used to configure the software.  A script will be provided to execute post configuration testing within the installation packages.

#### Success Conditions

### Initial Operational Testing

#### Purpose

The purpose of this testing is to test that the system has correctly started up after the initial installation step.  This testing is to be done after configuration testing is complete and the system has been started up based on documentation provided by developers.

#### Testing Strategy

The developers will provide a package called ```madlet-status-scripts```.  This package contains a series of scripts that can be utilized in order to validate the initial installation and validate the initial startup of the software.  The script ```/opt/madlet/bin/control``` can be run as follows in order to validate as root and run the initial startup:
```
/opt/madlet/bin/control --restart
```

Then as any other non-privileged user in the ```madlet``` group, the following can be run
```
/opt/madlet/bin/control --test --sensor aats
/opt/madlet/bin/control --test --sensor jaws
/opt/madlet/bin/control --test --sensor custody_raven
/opt/madlet/bin/control --test --sensor tactical_raven
```

#### Success Conditions

If each of the test returns a successful condition, then the entire objective is successful.  The possible failures can occur:
* AATs 
* JAWS
* Custody Raven
* Tactical Raven

In each case, this implies that the services do not have access to the data source that it is meant to access.  Please verify that the environmental testing objective and configuration testing objective have successfully passed.

### Operational Testing

#### Purpose

The purpose of this testing is to verify that during the operational lifetime of the software it is performing as expected.  The minimum operational capabilites and performance baselines are provided here, as well as testing strategies for verifying that the software is performing as expected.  In addtion to formal testing plans, exploratory testing to determine edge case issues and special cases will be outlined here.  This testing should occur on a recurring basis, as well as after any post-maintenence of the host system.

#### Testing Strategy

Several strategies can be employed to verify that, during operations, the software is operating as expected.

1.  Run ```/opt/madlet/bin/control --test --sensor <sensor>```
2.  Exam journalctl for each service.  The script ```/opt/madlet/bin/control``` provides a log option
  1. ```/opt/madlet/bin/control --log http_api```
  2. ```/opt/madlet/bin/control --log tensorflow_api```
  3. ```/opt/madlet/bin/control --log satnet-results-server```
  4. ```/opt/madlet/bin/control --log s4_client@aats```
  5. ```/opt/madlet/bin/control --log s4_client@jaws```
  6. ```/opt/madlet/bin/control --log fits-directory-reader@custody_raven```
  7. ```/opt/madlet/bin/control --log fits-directory-reader@tactical_raven```
  
  This examiniation should probably be done in conjunction with developer input/observation.  Project management and system administration should arrange for terminal access to the system.
3. Validate all services status api's are available and responding.  Normally this can be done via the ```/opt/madlet/bin/control``` script but this can also be individually done via standard unix commands.  In addition, the command ```ss``` can be used to verify all services are listening on status ports.
4.  running ```nvidia-smi``` utility to validate that the tensorflow_model_server is running correctly.

#### Success Conditions


## Test Management 

In addition to testing strategies outlined here, tools for recording outcomes of testing, planning failure resolution, and implementing risk mitigation strategies will be outlined here.

### Tools and Usage

#### Gitlab

### Risks and Mitigation

The following section lists strategies for mitigating potential risk involved with testing the system.

#### Terminal access to the system

This risk describes the possiblity of testers, users, and/or developers not having terminal access to the system.  Since the system itself is operated mostly autonomously, operationally this is not a risk.  The risk is during testing, unable to verify success or failure conditions within the system via the terminal.  Assuming this risk does not apply to system administrators, mitigation to risk can include ensuring that system administrators can export system configuration and log files from the system, and provide them to the developers and testers to verify success and failure conditions.

#### Restricted network configuration

This risk describes the possiblity of a highly restricted network connected to the system.  This includes restrictions on incoming and outgoing request, highly restrictive firewalls, and blocking of outgoing request from the system, either through the internally configured firewall, or through any network routing device that the system is connected to.  The software itself requires the ability to send messages to a dependent external server, specifically the SUMP server via UDP.  If this restriction occurs, half of the available data processing will be unavailable to the system, and all post processing capabilites will be disabled.  The mitigation to this is to verify that network access to the required third-party server is accessible from the system, and to negotiate access to the required system before system configuration occurs.  This can be done through software installation documentation that is made available to the system administrators and system security managers.

#### Inability to install required third-party softwares

This risk describes the possiblity of being unable or prevented from installing the required software.  Several sub categories of this risk include 

* the system's base configuration is unable to support the software

In order to mitigate this risk, developers will catalog the capabilities of the target system in order to validate up to a certain extent whether or not the base system is able to support the required software.

* the third-party software interferes with existing user software on the system

In order to mitigate this risk, software will be installed in such a way as to mitigate this risk through custom installation locations and configuration installed software packages to use these custom locations

* the software is restricted from being installed by either security or system administrators

In order to mitigate this risk, developers and project management will work with security and system administration to ensure that all software packages meet minimum security and configuration expectations to be installed.


#### Restricted administrative support to physical systems

This risk describes the possiblity of during testing not having a line of communication to system administrative support.  In order to mitigate this risk, a schedule should be made that all required parties agree to be available during testing, and documentation should be made available to all stakeholders regarding how to get in contact with system administrators in case of failure conditions during testing.

#### Restricted access to needed data

This risk describes the possiblity of being unable to access the required sensor data needed during operational testing.  In order to mitigate this risk, developers and project management should work with system and site administration to ensure that before the system moves to operational status, the system is configured as described in conops documentation to allow the software to have access to the data sources.  In the event that the data sources become unavailable during system operation after it was available during system startup, system and site administration should be available during system operation to ensure successful testing.

#### Data platform differs from expectations

This risk describes the possiblity that the sensor sources that produce the data operate differently from the expected way that it was simulated on the testing and development systems.  To mitigate this risk, developers will survey that system to ensure that the sensor data sources are available to the system before the software is installed, and should have strategy for verifying that the sensor data sources operate as expected on the system.

#### Restricted system configuration from expectations

This risk describes the possibility that the system itself is differs from the expected configuration.  This is mitigated by the pre-installation testing phase.  This risk will also be mitigated by providing documentation on expected system configuration to system and site administrators in order to correct for any difference between the testing/development platform, and the target production/testing platforms.

#### Restricted or blocked access to needed log or configuration files

### Roles and Expectations

The following section describes the roles and expectations of the people involved with testing the system.

#### Developers

Developers are expected to support any testing activities that are performed on the system, as well as provide documentation for configuring, maintaining, updating, and using the software.  During testing, failuure conditions of the software must be reported to developers and project management via standard ticketing channels.  

#### Testers

Testers are expected to review the test plan.  They must have a through understanding of how to operate the software that they are testing, as well as individual strategies for testing aspects of the system other that the one s listed here.  Testers are expected to be able to recognize when a success condition occurs for each phase of testing, as well as report failure conditions that occur during testing.  They must understand the proper channels needed to report failures.

#### System Administrators

System administrators are expected to provide support for each aspect of the testing phases.  They expected to have access or information regarding external dependency that were tested for during the first phase of the testing, as well as information regarding the physical system in which the MADLET software was installed on.  Questions regarding system configuration and system environment should be redirected to system administrators by the developers and testers.

#### Project Management
