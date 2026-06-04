# Ubuntu Security Lab

A practical Linux security lab for learning server administration, hardening and basic infrastructure security.

This repository documents the setup of a small Ubuntu server with real commands, real outputs and short learning notes.

## Goal

The goal of this project is to build and document a secure Ubuntu server step by step.

Focus areas:

* Linux server basics
* SSH key authentication
* sudo user management
* UFW firewall
* Fail2Ban SSH protection
* SSH hardening
* Docker installation
* nginx and web deployment later

## Why this project exists

I use this repository as a hands-on learning lab.

Instead of only reading theory, I configure a real server, check the results and document what each command does.

## Current setup

* Ubuntu server
* Non-root admin user
* SSH key login
* Direct root SSH login blocked
* UFW firewall enabled
* Fail2Ban active for SSH
* SSH hardening rules applied
* Docker installed and tested

## Documentation

* [User and SSH setup](docs/user-ssh-setup.md)
* [Firewall setup with UFW](docs/firewall-ufw-setup.md)
* [Fail2Ban SSH protection](docs/fail2ban-ssh-protection.md)
* [SSH hardening](docs/ssh-hardening.md)
* [Docker installation](docs/docker-installation.md)

## Skills demonstrated

* Linux command line usage
* User and permission management
* SSH access control
* Firewall configuration
* Service checks with systemctl
* Security logging and protection
* Docker repository setup
* Technical documentation in Markdown

## Status

Work in progress.

Next steps:

* Run nginx in Docker
* Create a shared project folder
* Add Docker Compose
* Deploy a small web application
* Add monitoring and logs
* Build a small security test lab
