# Network Protocol Simulation Tool (NetPST)

## Installation Requirements

To install the NetPST, you will need (as a minimum):

* VM with one core and 0.5G RAM (I believe AWS freei-tier EC2 instance will do)
  * 32-bit support is required
* For graphics, you will need 1280x1024 resolution to view packet captures
  * Some kind of graphics manager (X windows or any other will do)
* Since NetPST relates to networking, having 1 or more Ethernet ports is useful
* NetPST runs on Centos 6.5 Desktop and Ubuntu Desktop
  * Higher versions will work in 32-bit mode
  * A containerized version can run on any hardware supporting docker
* Root priviledges are required to send/receive raw Ethernet packets

## A Brief Description of NetPST

The Network Protocol Simulation Tool (NetPST) is a software tool designed for testing network
protocols (with a specialty in Internet routing protocols).  It is a [tcl/tk](https://www.tcl.tk/about/)
interpreter enhanced with APIs and libraries that allow a user to simulate network protocols and
configure networking devices.  NetPST is unique in its facilities for being able to write automated
test scripts for simulating different kinds of protocol behavior (e.g., normal "happy path" and
negative testing).

## What NetPST contains

* Off the shelf software components:
  * tcl8.3, tk8.3
  * itcl3.1, itk3.1 (object oriented tcl/tk)
  * expect5.31
  * libreadline-based CLI NetPST can also be loaded as a tcl package using the tcl "package require"
    command.
  * Network packet encoders/decoders for over 550 protocol fragments. Layer 2, routing, and
    multicast protocols: Ethernet, LLC, ARP, PPP, LCP, IPCP, RIPv1, RIPv2, OSPFv2, BGP4, IGMP,
    DVMRP, PIM, DHCP, SNMP, TFTP, IP, IP Options, TCP, TCP Options, ICMP, ISIS, Integrated ISIS,
    DOCSIS BPI & BPI+, MPLS, RSVP.
  * Finite State Machine framework (including support for timers, timeout events, packet filters,
    packet events, named events) for sending and receiving packets and building complex FSMs and
    protocol simulations
  * Test Execution Manager for building, running, logging, reporting pass/fail status for automated
    test scripts written in tcl
    * Test template generator
    * Test template generator for protocol test that uses an FSM
    * All tests can send/receive packets using any Linux supported Network Interface Card (NIC)
  * Realtime Monitoring and decoding for Ethernet and Sonet and any other Linux supported Network
    Interface Card (NIC)
  * Ability to capture packets in Monitor, change fields, and resend the packets.
  * Simulators for various routing protocols:
    * BGP4 router simulator: simulate either one peer or multiple peers all on one Ethernet.
      This has been used to test the limits of carrier class routers (e.g., Juniper) sending
      over 1 million NLRIs and simulating over 250 BGP4 peers
    * OSPF router simulator:  simulate either one OSPF peer or multiple peers all on one Ethernet.
      This has been used to test scaling (e.g., over 100 OSPFv2 peers and over 30,000 routes in the
      RUT's OSPF database all running overnight)
    * RIP router simulator for RIPv1 and RIPv2
    * Multicast Testing
      * IGMP speaker simulates a host that does IGMP Joins and Leaves; you can have multiple IGMP
        speakers running simultaneously
      * DVMRP router simulator simulates DVMRP speaker that does DVMRP probes, grafts, prunes, and
        report packets.
      * MFlow for generating multiple multicast packet flows
    * ARP simulator will allow one to simulate an Ethernet host that answers ARP (used for testing
      packet forwarding and to answer ARP request for simulated OSPF speakers).
  * Online help
    * Each object has its own help information
    * Help on each command available at the Command Line
    * Documentation available in HTML form
  * Test Suites Written By Users of NetPST
    * UNH IOL RIP suite (automated)
    * UNH IOL BGP4 suite (automated)
    * Some test cases for UNH IOL OSPF suite
  * Documentation in the form of HTML pages and online help that comes with the software.

All functionality that requires fast execution speed (e.g., encoding/decoding packets,
running FSMs, etc.) is all written in C/C++.  Tcl/tk/itcl/itk serves as a front end for
this functionality.  This gives you the flexibility and simplicity of an interpreted scripting
language plus the speed and efficiency of a compiled language.
NetPST has enough functionality to allow a user to write a full implementation of a routing
protocol using tcl scripting.  However, because the purpose of NetPST is for testing, you
only need to implement that small subset of the routing protocol that is big enough to make
a Device Under Test or Router Under Test (DUT or RUT) "think" that it is connected to a
router running a full implementation of the routing protocol.

Each command described can be executed on the NetPST CLI (Command Line Interface) or executed
from within a test script.  It is recommended that you experiment with commands using the CLI
until you get them to work as needed and then incorporate them into a test script.

## Summary

There are four primary areas of functionality and four primary commands: NPktIface, NPacket,
NFSM, and NTestMgr.  These are for creating:

  * Packet Interfaces: Used to send/receive data in the form of packets
  * Packets: Used to get fields, modify fields, and display packet information
  * FSMs: Used to simulate complex protocol state machines
  * Test Execution Manager: Used to automate running of test scripts in the form of test suites

Each one starts with an "N" to distinguish it from the tcl built-in commands.
NetPST was written and developed on Linux using gnu C/C++ tools.
