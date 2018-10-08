# Corda Custom Network Map Guide

This is a brief guide introducing you to the steps necessary to set up our [Corda Custom Network Map](l i n k    m i s s i n g    f e e l i n g s). We first run over the generic configuration instructions and then delve into a specific step-by-step example.

# Preparing the Configuration Bundle

To start a custom network map you need to provide a configuration bundle to the template. At the moment, we accept this bundle as a 7-zip file, encrypted with a password for validation & safety purposes. 

## 1. Certificates

First you need to prepare production-ready certificates. For the bundle, this will include the trust store, and the signing private key for the network map, both as a JKS and generated as per [Corda's Permissioning Page](https://docs.corda.net/releases/release-V3.1/permissioning.html). You will also need the node certificates to use the map.

For convenience, we provide a separate project that can generate these certificates for you: the [Corda Certificator](https://github.com/BCSTech-CordaTeam/cordaCertificator). Please refer to instructions at that repository for further information.

Gather the `truststore.jks` from any of the nodes as well as the `networkmapstore.jks` from the root folder, and place them into a folder named `certificates`

## 2. Contracts

Any contracts which you wish to use on the network must be whitelisted. To easily generate this whitelist, you can simply place  .jar files containing your contract classes in a folder named `contracts`.

## 3. Notaries

Any nodes which are to be used as notaries must also be defined ahead of time. To do this you must first generate the nodeInfos for any notaries on the network, then place them in a folder named `nodeInfos`.

MORE TO COME!
