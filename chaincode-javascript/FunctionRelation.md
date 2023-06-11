# Instructions of scripts

### codeStyles
- Nothing special

### application-javascript
#### html
- Nothing special

#### plugins
- functionHandler.js
  - Def the functions of transitions
- webhookHandler.js

#### **orgipetrinet_config.json**
- Define Petri nets (the workflow)
- Assets of all the parties: places, tokens, transitions
> Currently, the last transition is generate authorization token. **TODO: amend this transition**

- The concrete functions of transitions are defined in "functionHandeler.js", "const f"

#### **app.js**
- the "main" function for deploy petriNet on server (create all the assets) and run
- Define events in "eventHandler(ctx, event)"
  - Define events: CompleteTransition, PutToken, RemoveToken, Fire, etc
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

