<!--
===============================================================================
EchoForge Contributing Guide
-------------------------------------------------------------------------------

File:
    contributing.md

Purpose:
    Provides guidelines for contributing to the EchoForge project.

    This document describes coding standards, documentation expectations,
    development workflow, issue reporting, pull requests, code review, and
    project conventions.

    All contributors are encouraged to read this document before submitting
    changes.

Status:
    Living Contributor Guide

Created:
    2026-06-29

License:
    Apache License 2.0

===============================================================================
-->



Maintainer:
    EchoForge Contributors

Repository:
    https://github.com/chronomicron/echoforge

This document is part of the EchoForge project and should be kept in
sync with the implementation whenever possible.

Plugin Rule #1

Plugins must not

modify project metadata
call other plugins
create project directories
write database records

They only compute.