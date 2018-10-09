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

# An Example

1. Generate a new set of certificates by downloading and building the [Corda Certificator](https://github.com/BCSTech-CordaTeam/cordaCertificator), then running `java -jar build/libs/cordaCertificator-0.1-SNAPSHOT.jar -nd newNodes -nn "O=PartyA,L=London,C=GB" -nn "O=PartyB,L=New York,C=US" -nn "O=Notary,L=London,C=GB" -rg newRoots -rn "O=ARoot,L=Sydney,C=AU"`. For this particular version of the template your passwords will need to be `truststore`.
 
2. Download the [Yo Cordapp]() sample, and run `gradlew deployNodes` to get the basic node directories up.
 
   1. In each node directory, remove the additional-node-infos directory, the network-parameters file, and the current nodeInfo file
   
   2. Replace the certificates directory with the certificates generated in step 1.
   
   3. At the bottom of each node.conf, add the following:
   ```
   keyStorePassword="truststore"
   trustStorePassword="truststore"
   compatibilityZoneURL="http://localhost"
   devMode=false
   ```
 
3. In the notary directory, run `java -jar corda.jar --just-generate-node-info`.

4. As per the instructions above, prepare the configuration bundle: Use certificates that you have generated, the jars in the cordapps folder, and the notary's nodeInfo file.

5. Use our solution to deploy a network map with the above configuration bundle.

6. In each node.conf, replace the compatibilityZoneURL with a line similar to `compatibilityZoneURL="http://<VM-DNS>:8080"`

7. Start the nodes as per the instructions in yo-cordapp and test that they work!
