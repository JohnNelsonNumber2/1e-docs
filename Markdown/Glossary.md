# Glossary
This page contains a glossary of terms and abbreviations that may be seen in the SCALE documentation which need defining.

## 1E
The company who created the 1E Platform, formerly known as Tachyon

See [1e.com](https://www.1e.com)

## 1E Client
a.k.a 1E Client, 1E Agent
 
This is the service that executes instructions and automations--which were written in SCALE--and runs at the endpoints under the `root` or `SYSTEM` context.

## 1E Platform
a.k.a Tachyon Platform

This is the SaaS or on-prem back-end that hosts the 1E Platform, formerly known as Tachyon.

## Agent
See [1E Client](#1e-client)

## Automation
a.k.a Fragment

An automation is a `PreCondition`, `Check` or `Fix` in `Endpoint Automation` (formerly known as a `Guaranteed State` fragment).

Sometimes people use instruction to mean both instructions and automations created with TIMS, but to be precise, an automation is PreCondition, Check or Fix.

See also: [Instruction](#instruction)

## Client
See [1E Client](#1e-client)

## CR
Carriage-return

This is the carriage-return (ASCII-13) character. This is the first half of the string that gets put into a text editor when you type `ENTER` in Windows

## CRLF
Carriage-return-line-feed
 
This is the equivalent of hitting `ENTER` on your keyboard which puts the carriage-return (ASCII-13) and line-feed (ASCII-10) characters in the editor in Windows

## Instruction
A `Question` or `Action` in `Endpoint Troubleshooting` (formerly known as `Explorer`) is an instruction.

Sometimes people use instruction to mean any instruction or automation you create with TIMS, but to be precise, an instruction is a question or action.

See also: [Automation](#automation)

## LF
Line-feed

This is the line-feed (ASCII-10) character. This is the character that gets put into a text editor when you type `ENTER` in Linux, or the last half of the `CRLF` character in Windows

## On-prem
"On-prem" or "on-premises" refers to software, hardware, or computing resources that are managed, maintained, and operated within an organization's own physical premises, rather than through a cloud computing service or a third-party provider.

The [1E Platform](#1e-platform) has an on-prem (self-hosted) option

## SaaS
SaaS, or Software as a Service, is a software distribution model where applications are hosted by a service provider or vendor and made available to customers over the Internet. 

The [1E Platform](#1e-platform) has a subscription-based (cloud-hosted) option

## SCALE
**S**&#8239;imple<br>
**C**&#8239;ross-platform<br>
**A**&#8239;gent<br>
**L**&#8239;anguage for<br>
**E**&#8239;xtensibility<br>

`SCALE` is the automation language used by the [1E Client](#client).
It is a superset of the syntax provided by [SQLite](#sqlite) so it's very easy to learn if you already know the SQLite TSQL syntax

## SQLite
SQLite is an in-process, self-contained, serverless, zero-configuration, transactional SQL database engine utilized by the [1E Client](#client)

See [sqlite.org](https://sqlite.org) for more information

## Switch
The Switch is a component of the [1E Platform](#1e-platform) which handles communication with the clients.  It uses an always-on connection which is optimized for a fast "one-packet" architecture where the payload can be sent to the client in a single network packet and get a response in a single packet. Instructions are sent to clients from the platform and responses are returned from instructions using the switch.  

## Tachyon
See [1E Platform](#1e-platform)

## TIMS
**T**&#8239;he<br>
**I**&#8239;nstruction<br>
**M**&#8239;anagement<br>
**S**&#8239;tudio<br>

also known as

**T**&#8239;achyon<br>
**I**&#8239;nstruction<br>
**M**&#8239;anagement<br>
**S**&#8239;tudio

TIMS is the IDE with which [instructions](#instruction) and [automations](#automation) used by the 1E platform are written.
