# Instructions of scripts

### codeStyles
- Nothing special

### application-javascript
#### html
- Nothing special

#### plugins
- functionHandler.js
  - Def the functions of actions of transitions
- webhookHandler.js

#### **orgXpetrinet_config.json**
- Define Petri nets (the workflow)
- Assets of all the parties: places, tokens, transitions
> Currently, the last transition is generate authorization token. **TODO: amend this transition**

- The concrete functions of transitions are defined in "functionHandeler.js", "const f"

#### **app.js**
- the "main" function for deploy petriNet on server (create all the assets) and run
  + socket: connet to the webserver
  + **console**: the hyperledger
- Define events in "eventHandler(ctx, event)"
  - Define events: CompleteTransition, PutToken, RemoveToken, Fire
  - After event accomplished, emit the message from the queue
- Lots of constant are defined in other files "'../../test-application3/javascript/CAUtil.js'"
- Environmental files?

#### api.js
- sync with the webpage, for demo
- Organizations on block chain are built by functions within Hyperledger Fabric
> An concrete example: https://zhiqich.github.io/2021/03/06/hyperledgerfabric/#Sample-Application
+ Line 41
```
buildCCPOrg = (options['org-number'] == 1) ? AppUtils.buildCCPOrg1 : AppUtils.buildCCPOrg2
```
+ Line 161-208: Function initGatewayForOrg

- Line 269-280, completeTransition: show the log in console about submitting the transaction that needs to be completed

- Secret might be in "PutRemoveTokens" in petrinet.js and app.js

#### Questions
- Where does function **getKeys** is used?
- Function **loadModulers**, do not know what is this function for
- Function **initAmqp** seems unused as well, amqp is replaced by mqtt?
- Function **checkAsset** seems unused, colored Petrinet?
- In petrinet.js, Line 313, the function of **completeTransition** is realized by **getAssetJASON** + **if judgement**
- Function **createAndMoveToken** seems unused.
  > In petrinet.js, there is a function **PutToken**
- app.js, Line 635, what is "event"?


#### run_orgi.sh
- the command of running

#### web.js (This one seems not for work4, but work1)
- amqp, the network layer

#### Questions
- where is the blockchain layer?
- todo: need to download other additional files

# Ideas
### Define Petri nets assets in org1petrinet_cionfig.json
> places, transitions(actions "cmd"), token (Personal token to activate, auth-token), arcs(id of other assets)

### Define events of Petri nets in functionHandler.js
> CompleteTransition, PutToken, RemoveToken, Fire; Emit to the **console**

### With network-lab/mqtt_listener/mqtt_listener.js
> listen to the console, execute the corresponding "cmd" through mqtt

### chaincode-javascript
> The `chaincode-javascript` folder implements a library for running a petrinet. So that is accepting tokens, moving them, creating them, starting transitions, etc

#### petrinet.js (Petrinet extends Contract)
> At the very least, this reads the org{1,2,3}petrinet_config.json files and create a new petrinet instance => Line 119
> Most likely, it also takes this petrinet instance and runs it, creating the events used in `application-javascript` and by the network repository to run the `cmd` in containers
##### Define the class of Petrinet (Line 118+) and help functions (Line 118-) 
- const events records the remarks
- //TODO: there should be a webpage container that using help function "GenerateToken"
##### Methods of class Petrinet
- PutToken
  - Line 190: put the token into place _i_
  - Line 208-211: find the arcs set whose source place is _i_
  - Line 290-295: fire the transitions: by "firedTransitions"
  - Line 271: update blockchain state by "ctx.stub.putState"
  - Return the transitions that are firing
- CompleteTransition
  - Line 328-355: Remove the token from the inplace which triggers the transition
  - Line 358-404: Put the token at the outplace of the transition 
    - Line 372-386: **generate token**. //TODO: create a new token "Vote done"
  - Line 406-472: record the transition that is to be fired
  - Line 480-505: record the fired transition
    > In petrinet.js, "Fire" is pushed into events(hook with the application) and console(blockchain).
    > In app.js, eventHandler(ctx, fireEvent) is executed when listen to "Fire", and if fail, will push "err" in console.log
    > In petrinet.js, if not find "err" when listening, then record the event
- CreateFunctionTransition
  - Function transitions can transfer data through "params"
  - TODO: make use of this function to record vote results
##### Questions
- Why the number of token in a place is limited to one, line 40, function "isSpaceInPlace"
> The assumption of Petrinet 
- There must be a def of place and token, Xin hasn't found it yet. Line 173, toke.type !== place.type; line 182: token.reuse; line 184: token.status;
> Should be within the package of "fabric-contract-api"
- Very important function: line 190 place.tokens.push(tokens). Where was it defined?
> Should be within the package of "fabric-contract-api"
- Line 265-269, do not understand how to use token to transfer data
> Tokens themselves contains data (helpFun "Generate token")
- In the Def of Petrinet, where does function "GenerateToken(...)" is used?
- Line 287 and 288, connect to apps.js, but not very clear
- Line 843-865: CreateWebhookTransition
  - Used in Line 535 of app.js
  - Don't know what is the actual function of it

