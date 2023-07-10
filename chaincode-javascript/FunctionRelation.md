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

#### run_orgi.sh
- the command of running

#### api.js
- sync with the webpage, for demo

#### web.js
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
##### Methods of class Petrinet
- PutToken
  - Line 190: put the token into place _i_
  - Line 208-211: find the arcs set whose source place is _i_
  - Line 290-295: fire the transitions: by "firedTransitions"
  - Line 271: update blockchain state by "ctx.stub.putState"
  - Return the transitions that are firing
- CompleteTransition
  - 
- CreateWebhookTransition
- CreateFunctionTransition

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

