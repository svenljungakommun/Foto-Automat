# Foto-Automat Självservice v3.0.1 ((SSO-based Internal Application))

** ASP.NET MVC application for secure employee ID photo capture using SSO, AD integration, and approval workflows.** 
Maintainer: Svenljunga kommun  (Abdulaziz Alamzrli)

------

# Key Principles

* No direct authentication logic

* Gateway-driven identity

* Claims-based authorization

* Separation between identity, business logic, and infrastructure

* Designed for internal / municipal environments

------

# Architecture Overview
Browser
  ↓
SSO Gateway (injects identity headers)
  ↓
ASP.NET MVC 5 Application (OWIN)
  ↓
SQL Server

------

# Authentication Model

Authentication is handled outside the application

The SSO gateway injects identity headers (e.g. x-ID)

* The application:

 * Reads headers

 * Normalizes the identity

 * Issues an authentication cookie

 * Works with claims internally

No passwords are handled or stored by the application.

------

# SSO Middleware Responsibilities

The custom OWIN middleware is responsible only for:

 * Reading identity headers from the request

 * Creating a ClaimsIdentity

 *  Issuing an authentication cookie

It does not:

 * Validate passwords

 * Communicate with Active Directory directly

 * Perform authorization decisions

------

# Identity & User Data

* User identifier: AccountName

* Additional user metadata (display name, title, photo) is resolved via:

 * Active Directory (read-only)

 * SQL Server (order-specific data)

Identity enrichment is cached per request.



------

# Application Features
View active photo orders

* Capture photo via browser camera

* Preview and approve captured photo

* Optional PIN code selection (separate step)

* Automatic session handling via SSO

* Clean logout flow via central auth system
  
------

# Database

* SQL Server

* Read/write access via a dedicated service account

* No end-user database credentials

* All data access is parameterized

Example responsibilities:

* Store photo orders

* Store captured images

* Track workflow state (captured, approved, etc.)

* Feature toggles via system tables

------
# System Control
The application supports runtime enable/disable of functionality via database flags.

Example:

* Table: EnSystemFunctions

* Columns:

* functionName

* functionEnabled

* btnEnabled

This allows central IT to enable/disable systems without redeployment.
------
# What This Project Is NOT

* Not a generic authentication system

* Not a public-facing application

* Not intended for consumer use

* Not a replacement for an identity provider
  
------
# Security Notes

* No credentials stored in code

* No passwords transmitted through the app

* Anti-forgery protection enabled

* Claims-based identity enforced

* Designed for restricted internal networks

  ------

  # Project Status
  
* Status: Active

* Usage: Internal / Enterprise

* Deployment: On-prem / private infrastructure

* Maintenance: Best-effort

------

  # License

This project is provided as-is for internal and educational use.

------

  # Final Note

This repository focuses on architecture, separation of concerns, and secure internal workflows rather than being a plug-and-play solution.

------
