# Vagrant YAML Template

## Introduction

Template to define multiple Vagrant machines with a YAML file. This template allows adding multiple public keys to the vagrant user in the VMs.

Tested with VMWare Workstation provider, VirtualBox should works too (can't test properly yet, VBox with WSL2 is slow asf).

## How To Use

1. Copy file `vagrant-config.yml.example` to `vagrant-config.yml`.
2. Edit `vagrant-config.yml` as you need.
3. Run `vagrant up`
