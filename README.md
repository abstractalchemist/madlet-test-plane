MADLET Test Plan
=======

## Introduction

The purpose of this document is to explain the objectives, targets, and strategies for validating and testing the installation of the MADLET software.  The testing plan is to be executed at each phase of the MADLET's software operational lifecycle.  This document is will explain the purpose of this testing, a strategy for testing each phase, the conditions under which the testing phase is considered a success, roles under which testers, developers, and user fall under, and risk mitigation strategies.

## Assumptions

The following documents the assumptions under which the stakeholders, which include the testers, developers, and users will be operating under during each testing phase.  Risk mitigation strategies should these assumptions fail will be documented under the risk mitigation section.

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

the purpose of this testing is to validate the software was correctly configured after the MADLET software has been correctly installed.  This testing is to be done after the MADLET software has been validated to have been installed on the system. 

#### Testing Strategy

The system administrator and/or the software developers, possible following a form of DevOps strategy, will have access to all information from the first objective.  The person configuring the software will have access to the documentation needed to correctly configure the software.  The tester will also have full access to the documentation that was used to configure the software.  The tester will validate the following:

#### Success Conditions

### Initial Startup Testing

#### Purpose

#### Testing Strategy

#### Success Conditions

### Operational Testing

#### Purpose

#### Testing Strategy

#### Success Conditions

## Test Management 

### Tools and Usage

### Risks and Mitigation

### Roles and Expectations
