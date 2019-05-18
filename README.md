MADLET Test Plan
=======

## Introduction

The purpose of this document is to explain the objectives, targets, and strategies for validating and testing the installation of the MADLET software.  

## Assumptions

## Objectives

### Environmental Installation Testing

#### Purpose

The purpose of this testing is to validate the environment in which the MADLET software will be installed.  

#### Testing Strategy

The tester will validate the following:

* the operating system in which the system is installed will be a Red Hat 7 operating system
   * this objective can be verified by running ```cat /etc/redhat-release ```.  The value must be **Red Hat Enterprise Linux Server release 7.6 (MAIPO)**

* the system in which the MADLET software will be installed will have a route to a SUMP server
* the system has access to the AATS and JAWS sensor data through a NAS, or some equivalent data
* the system has access to the Custody and Tactical Raven sensor data through some form of a network mounted data source
* the physical system that this software is installed on has GPU capability

#### Success Conditions

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
