# TARDIS Hyperledger Fabric Sample Code

Please follow [this blogpost] about it, there's a full explanation on how to use it.

Should you have questions, please feel free to leave comments in the comment section of the post.

## Getting Started

This repo holds the code that describes the business network where the system will operate. It holds models,
access control lists and business logic operations.

In order to install all of the prerequisites, you'll need to run these commands:
```bash
$ npm install -g composer-cli
$ npm install -g composer-rest-server
$ npm install -g generator-hyperledger-composer
$ npm install -g yo
```

If you use VSCode, you can look for the `Hyperledger Composer` extension from the Marketplace.

You'll also need to download and install docker, which you can do by going to [the Docker store] page,
and downloading the Docker Community Edition for Mac.

Once you've installed Docker, you can go ahead and get Hyperledger Fabric by running these commands:
```bash
$ mkdir ~/fabric-tools && cd $_
$ curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.zip
$ unzip fabric-dev-servers.zip
$ cd fabric-dev-servers && ./downloadFabric.sh
```

The first time you run the Hyperledger Fabric servers, you should do it like this:
```bash
$ ./startFabric.sh
$ ./createPeerAdminCard.sh
```
So you get the peer admin card created. Again, that last command is supposed to be run only once.

After this, you can `./stopFabric.sh` and `./startFabric.sh` accordingly for your next dev sessions.

## Building for deployment

For deployment, navigate to this project's folder and run:
```bash
$ composer archive create -t dir -n .
```
This will run the compiler and lint the project as well, plus will create a `.bna` file which is the package
that has all of the files to be deployed.

## Deploying to local environment
After you've created the corresponding `.bna` file, you should be able to install the business network through:
```bash
$ composer runtime install --card PeerAdmin@hlfv1 --businessNetworkName tardis
```
In case you've already run this first command and just need to update the deployment, you can just run:
```bash
$ composer network update -c admin@tardis -a tardis@0.0.1.bna
```

Next up, we need to deploy our `.bna` and create a network admin card:
```bash
$ composer network start --card PeerAdmin@hlfv1 --networkAdmin admin --networkAdminEnrollSecret adminpw --archiveFile tardis@0.0.1.bna --file networkadmin.card
```
Make sure you change the parameters accordingly, for instance the name of the `.bna` file may change according to the version of the project.

Now, we need to import the generated `networkadmin.card` card file by typing:
```bash
$ composer card import --file networkadmin.card
```

To check that everything's been set up correctly, we're could run a `ping` command like so:
```bash
$ composer network ping --card admin@tardis
```

## The REST server

In order to run a local version of the REST server once you've already deployed the business-network, is by
running:
```bash
composer-rest-server -c admin@tardis -n never -p 9000
```

This should launch a REST API server instance in [http://localhost:9000/] which you can [explore] to play
around with the data.

## The Composer Playground

For demo purposes, or for just playing around with data, you might wanna run this composer playground locally. If so,
here's how:
```bash
$ composer-playground -p 5000
```

This should be all you need to run the playground locally. Have fun with it!


[this blogpost]: https://gorillalogic.com/blog/hyperledger-fabric-developer-tutorial-part-1-make-your-own-blockchain
[the Docker store]: https://store.docker.com/editions/community/docker-ce-desktop-mac
[http://localhost:9000/]: http://localhost:9000/
[explore]: http://localhost:9000/explorer
