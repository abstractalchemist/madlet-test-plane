MADLET Test Plan
=======

## Introduction

The purpose of this document is to explain the objectives, targets, and strategies for validating and testing the installation of the MADLET software.  

## Assumptions

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

* the following ports must be opened on the system
   * 

#### Success Conditions

This test objective must be complete fulfilled.  Each capability must be verified before software installation should begin.  A failure of any one of these sub objectives causes a complete failure of the entire objective.

### Post Installation Testing

#### Purpose

#### Testing Strategy

#### Success Conditions

### Configuration Testing

#### Purpose

#### Testing Strategy

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
