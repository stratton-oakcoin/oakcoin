Oakcoin Core integration/staging tree
=====================================



Start the docker container:

    docker run -i -t smatthewenglish/oakcoin:version0 /bin/bash

Lock conversation


# Excellent resource:

    https://bitcointalk.org/index.php?topic=372545.0



## RPC

    - doing a search for "-rpcport", yields some files. For sure these need to be modified/changed...







Foreword: No line numbers are given. You must use critical thinking to complete this guide. If you are unable to complete this guide, then programming is not for and you and you don't need to clone a cryptocoin anyway. there are far to many copy paste coins around already and your ideas are nothing special. if they were, you wouldn't be reading this guide.. surprise!

Step 1- Know thy source code.(required)
   -always read your sourcecode. its important that you gain atleast a simple understanding of what it does. the more time you spend reading code,     the better you become at writing it. study, practice, troubleshoot, ask questions in that order. No one has the time to cater to your ass, learn to do it yourself and be better for it in the long run. 

Step 2- Find & Replace coin names, and replace/customize images as desired.(required)
   -if you need help with this, you probably have no business here in the first place. this is not rocket science, but critical thinking is required. 
Step 3- Customize GUI with QT designer(optional)
   -blundertoe taught me how to use this tool, and its pretty intuitive. a bit confusing, but if you know your way around an xml style sheet, that's half the battle.
Step 4- Ports(required)

  a) bitcoinrpc.cpp 
        -change RPC port.

    
Code:
ip::tcp::endpoint endpoint(bindAddress, GetArg("-rpcport", 55883));

    
Code:
if (!d.connect(GetArg("-rpcconnect", "127.0.0.1"), GetArg("-rpcport", "55883")))

  b) init.cpp
       -change RPC port
    
    
Code:
-rpcport= " + _("Listen for JSON-RPC connections on  (default: 55883)") + "\n" +

       -change Testnet and Common port(P2P).

    
Code:
-port= " + _("Listen for connections on  (default: 55884 or testnet: 45884)") + "\n" +
 
  c) protocol.h

      -Testnet and P2P port again.

    
Code:
return testnet ? 45883 : 55884;
Step 5- Add a seednode(optional, but recommended)
   net.cpp
     -add a seednode using ip or dns name.
    
Code:
{"some website name", "somewebsite.org or ip x.x.x.x"},
Step 6- Set your coins parameters
   a)main.cpp
      -set your block rewards. once you've set the initial subsidy, you can set optional parameters in this scope, and use conditional statements. critical thinking required, but this is probably the easiest aspect of programming C++ as there is. you can do it. just try.
    
    
Code:
int64 nSubsidy = 1 * COIN;

     -if you want an example template for conditional rewards, here is an example from when i was working on OSC and needed some feedback from some friends in IRC. please be advised additional steps are required for Proof Of Stake coins. 
    Example
    http://pastebin.com/QMdAiGzP

   b)main.cpp
     -set target spacing and other vital coin parameters.pretty self explanatory. most source's this will be commented to help you understand what it means. if you are not used to seeing algebra or math in programming, it may look a bit confusing at first.
    
Code:
static const int64 nTargetSpacing = 120; 
static const int64 nTargetTimespan = 1 * 24 * 60 * 60; 

   c)main.cpp
     -you need to change your coins network id to make it unique. find this in your source. 

    
Code:
unsigned char pchMessageStart[4] =
     this will be followed by four  4 charachter hex codes(representing ascii charachters) inside a set of brackets. such as 
    
Code:
{ 0xe4, 0xe8, 0xe9, 0xe5 };
    -change a few of the hexadecimal codes, but make sure they are valid, eg. - 0xdd, 0xff, 0xfe, 0xf3, 0xda, 0xff, 0xle, etc.
  
   d)main.h
   - change first the number of total coins ever to be produced.
   find this:
  
Code:
static const int64 MAX_MONEY =
  and this:
  
Code:
return dPriority > COIN *
  e)base58.h
       -change the starting letter for the addresses of your coin. for a list of values, google base58 charachter codes, or better yet, see this link from the bitcoin wiki. https://en.bitcoin.it/wiki/Base58Check_encoding
  
Code:
PUBKEY_ADDRESS =
Step 7- Review, and complete additional steps.
   completing the bare minimum steps
    -once you've made it this far, its time to hop on over to this guide http://devtome.com/doku.php?id=scrypt_altcoin_cloning_guide and make sure you've done everything in this guide now
Step 8- Finishing touches


if (POS === true)
{
Do these steps to setup a checkpoint server:
assuming you've already compiled, open your client. open the debug window(or from the command prompt/shell if using daemon)
type
Code:
makekeypair

you will get an output like this

Code:

￼
{
"PrivateKey" : "308201130201010420a9a674c69c336384f86a0d7bf47b2248e95c2c0cd568cf6c20455c63990a2471a081a53081a2020101302c06072a8648ce3d0101022100ffffffffffffffffffffdddddffffffffffffffffffffffffffffffffffefffffc2f300604010004010704410479be667ef9dcbbac55a06295ce870b07029bfcdb2dce28d959f2815b16f81798483ada7726a3c4655da4fbfc0e1108a8fd17b448a68554199c47d08ffb10d4b8022100fffffffffffffffffffffffffffffffebaaedce6af48a03bbfd25e8cd0364141020101a1333342000408ebd69d4e1564efczzz0a77a876496713a1ggg58c47aeb3abb57e2ee83d72df0d73fe7f33d3dc33fa97502d961e7251fe12ccf0f9a15b2a96d7895078aed81c",
"PublicKey" : "0408ebd69d4e1564efcbee0a77a876496713a10ae58c47aebfffb57e2ee83d72df0d73fe7f33d3dc33fa97502d961e7251fe12ccf0f9a15b2a96d7895078aed81c"
}



save this in a text document. 

close the client.

open your configuration file for your coin(if you haven't created one, now would be a great time to start.). paste in your privatekey in the following format
Code:
checkpointkey=<your long ass hell private key here>
now save and exit, then restart the client.

now you must reflect this in the sourcecode, by adding your public key as the checkpoint master. without this, it will not work at all.
checkpoints.cpp
find this and add your public key( do not add your private key in the source, only in your conf file. 
Code:
const std::string CSyncCheckpoint::strMasterPubKey =
there will be a public key already there. delete it, and put yours in its place. now save, and recompile the clients. congratulations, you are finished.
}else{

//you're done!
}

















[![Build Status](https://travis-ci.org/oakcoin/oakcoin.svg?branch=master)](https://travis-ci.org/oakcoin/oakcoin)

https://oakcoincore.org

What is Oakcoin?
----------------

Oakcoin is an experimental digital currency that enables instant payments to
anyone, anywhere in the world. Oakcoin uses peer-to-peer technology to operate
with no central authority: managing transactions and issuing money are carried
out collectively by the network. Oakcoin Core is the name of open source
software which enables the use of this currency.

For more information, as well as an immediately useable, binary version of
the Oakcoin Core software, see https://oakcoin.org/en/download, or read the
[original whitepaper](https://oakcoincore.org/oakcoin.pdf).

License
-------

Oakcoin Core is released under the terms of the MIT license. See [COPYING](COPYING) for more
information or see https://opensource.org/licenses/MIT.

Development Process
-------------------

The `master` branch is regularly built and tested, but is not guaranteed to be
completely stable. [Tags](https://github.com/oakcoin/oakcoin/tags) are created
regularly to indicate new official, stable release versions of Oakcoin Core.

The contribution workflow is described in [CONTRIBUTING.md](CONTRIBUTING.md).

The developer [mailing list](https://lists.linuxfoundation.org/mailman/listinfo/oakcoin-dev)
should be used to discuss complicated or controversial changes before working
on a patch set.

Developer IRC can be found on Freenode at #oakcoin-core-dev.

Testing
-------

Testing and code review is the bottleneck for development; we get more pull
requests than we can review and test on short notice. Please be patient and help out by testing
other people's pull requests, and remember this is a security-critical project where any mistake might cost people
lots of money.

### Automated Testing

Developers are strongly encouraged to write [unit tests](src/test/README.md) for new code, and to
submit new unit tests for old code. Unit tests can be compiled and run
(assuming they weren't disabled in configure) with: `make check`. Further details on running
and extending unit tests can be found in [/src/test/README.md](/src/test/README.md).

There are also [regression and integration tests](/test), written
in Python, that are run automatically on the build server.
These tests can be run (if the [test dependencies](/test) are installed) with: `test/functional/test_runner.py`

The Travis CI system makes sure that every pull request is built for Windows, Linux, and OS X, and that unit/sanity tests are run automatically.

### Manual Quality Assurance (QA) Testing

Changes should be tested by somebody other than the developer who wrote the
code. This is especially important for large or high-risk changes. It is useful
to add a test plan to the pull request description if testing the changes is
not straightforward.

Translations
------------

Changes to translations as well as new translations can be submitted to
[Oakcoin Core's Transifex page](https://www.transifex.com/projects/p/oakcoin/).

Translations are periodically pulled from Transifex and merged into the git repository. See the
[translation process](doc/translation_process.md) for details on how this works.

**Important**: We do not accept translation changes as GitHub pull requests because the next
pull from Transifex would automatically overwrite them again.

Translators should also subscribe to the [mailing list](https://groups.google.com/forum/#!forum/oakcoin-translators).
