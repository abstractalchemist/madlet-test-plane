MADLET Test Plan
=======

## Introduction

The purpose of this document is to explain the objectives, targets, and strategies for validating and testing the installation of the MADLET software.  The testing plan is to be executed at each phase of the MADLET's software operational lifecycle.  This document is will explain the purpose of this testing, a strategy for testing each phase, the conditions under which the testing phase is considered a success, roles under which testers, developers, and user fall under, and risk mitigation strategies.

## Assumptions

The following documents the assumptions under which the stakeholders, which include the testers, developers, and users will be operating under during each testing phase.  Risk mitigation strategies should these assumptions fail will be documented under the risk mitigation section.  Assumptions specific to each testing objective will be described in the testing strategy under each objective.

It is assumed that the tester has physical access to the system in which the MADLET software will be installed.  The tester must have some form of administrative access to the system, such as the ability to sudo to the root user, the ability to transfer software to and from the system, and the ability to reconfigure the system as needed.

## Objectives

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

#### Success Conditions

### Configuration Testing

#### Purpose

The purpose of this testing is to validate the software was correctly configured after the MADLET software has been correctly installed.  This testing is to be done after the MADLET software has been validated to have been installed on the system. 

#### Testing Strategy

The system administrator and/or the software developers, possible following a form of DevOps strategy, will have access to all information from the first objective.  The person configuring the software will have access to the documentation needed to correctly configure the software.  The tester will also have full access to the documentation that was used to configure the software.  The tester will validate the following:

#### Success Conditions

### Initial Operational Testing

#### Purpose

The purpose of this testing is to test that the system has correctly started up after the initial installation step.  This testing is to be done after configuration testing is complete and the system has been started up based on documentation provided by developers.

#### Testing Strategy

#### Success Conditions

### Operational Testing

The purpose of this testing is to verify that during the operational lifetime of the software it is performing as expected.  The minimum operational capabilites and performance baselines are provided here, as well as testing strategies for verifying that the software is performing as expected.  In addtion to formal testing plans, exploratory testing to determine edge case issues and special cases will be outlined here.

#### Purpose

#### Testing Strategy

#### Success Conditions

## Test Management 

In addition to testing strategies outlined here, tools for recording outcomes of testing, planning failure resolution, and implementing risk mitigation strategies will be outlined here.

### Tools and Usage

#### Gitlab

### Risks and Mitigation

The following section lists strategies for mitigating potential risk involved with testing the system.

* Terminal access to the system
* Restricted network configuration
* Inability to install required third-party softwares
* Restricted administrative support to physical systems
* Restricted access to needed data
* Data platform differs from expectations
* Restricted system configuration from expectations
* Restricted or blocked access to needed log or configuration files

### Roles and Expectations

The following section describes the roles and expectations of the people involved with testing the system.

#### Developers

Developers are expected to support any testing activities that are performed on the system, as well as provide documentation for configuring, maintaining, updating, and using the software.  During testing, failuure conditions of the software must be reported to developers and project management via standard ticketing channels.  

#### Testers

Testers are expected to review the test plan.  They must have a through understanding of how to operate the software that they are testing, as well as individual strategies for testing aspects of the system other that the one s listed here.  Testers are expected to be able to recognize when a success condition occurs for each phase of testing, as well as report failure conditions that occur during testing.  They must understand the proper channels needed to report failures.

#### System Administrators

System administrators are expected to provide support for each aspect of the testing phases.  They expected to have access or information regarding external dependency that were tested for during the first phase of the testing, as well as information regarding the physical system in which the MADLET software was installed on.  Questions regarding system configuration and system environment should be redirected to system administrators by the developers and testers.

#### Project Management
