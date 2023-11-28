# Welcome to Ethscriptions VM!

### Introduction

On Aug 7, 2023 we proposed the Ethscriptions Virtual Machine (ESC VM), a new protocol built on top of Ethscriptions. The purpose of the ESC VM is to enhance the functionality and scope of the Ethscriptions Protocol by enabling it to function as a general computation engine.

Users access the VM by creating special ethscriptions that the VM interprets as computer commands to special computer programs called Dumb Contracts.

Read the rest of the proposal here:

{% embed url="https://docs.ethscriptions.com/esips/esip-4-the-ethscriptions-virtual-machine" %}

### <mark style="background-color:green;">The first Ethscriptions VM implementation is now Open Source</mark>

In addition to deploying and calling existing Dumb Contracts, you can use this technology to create your own Dumb Contracts **on Goerli**.

Our Ethscriptions VM implementation has two components:

1. [Ethscriptions VM API](https://github.com/ethscriptions-protocol/ethscriptions-vm-api): This is a Ruby on Rails app that ingests ethscriptions from an Ethscriptions indexer, executes Dumb Contract logic, and stores the result. All Dumb Contracts live in this app.\

2. [Ethscriptions VM Client](https://github.com/ethscriptions-protocol/ethscriptions-vm-client): This is a Next app that you can use to try out Dumb Contracts. Think of it a little like Etherscan. You can connect it to your local VM API instance or hook it up our VM API instance at [https://goerli-api.ethscriptionsvm.com](https://goerli-api.ethscriptionsvm.com).

You can also try out a hosted version of the app on [https://goerli.ethscriptionsvm.com](https://goerli.ethscriptionsvm.com).

### What do I do now?

These apps are not yet production-ready. We are launching them early in beta form to get feedback from the community so we can go live on mainnet as quickly as possible.

So please help us! Test the VM and submit feedback in GitHub Issues on each repo:

* [VM Client](https://github.com/ethscriptions-protocol/ethscriptions-vm-client/issues)
* [VM API](https://github.com/ethscriptions-protocol/ethscriptions-vm-api/issues)

The other thing you can do is to write Dumb Contracts! All Dumb Contracts will be contributed by the community so we are looking out for great submissions.

If you have one, or you have any proposal for changing the apps, please submit a pull request!
