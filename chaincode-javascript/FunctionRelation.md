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

#### petrinet.js
> At the very least, this reads the org{1,2,3}petrinet_config.json files and create a new petrinet instance
> Most likely, it also takes this petrinet instance and runs it, creating the events used in `application-javascript` and by the network repository to run the `cmd` in containers
