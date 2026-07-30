________________
note che non c'entrano nula
```
The

compose-parallel.sh

port probe doesn't work in this devcontainer.

It relies on

lsof -iTCP:<port> -sTCP:LISTEN

, which can't see Docker-published host ports from inside the container (reports busy ports as free), so it never shifts ports and always claims the defaults. The whole point of that script is to let stacks coexist on one host, so this is effectively defeated here. That's why the collision happened rather than an automatic shift. If you spin up a second concurrent stack it'll collide again. Want me to open a follow-up to make the probe fall back to

ss

/

/proc

when

lsof

can't see docker-proxy bindings? (

ss

is available and does see them.
```


___________________

`~/trial-simulator-apps/docs/CICD.md `

`ENGINEERING_ASSESMENT.md`


### Steps
1. [prima volta] Cancellare release da gitlab e creare branch release da GitLab (use the Clementoni interface)

#### per hotfix
1. banch hotfix
2. merge fi hotfix -> release
3. deploy
4. staccare nuovo branch su dev (ex b1)
5. mergiare release dentro a b1
6. mergiare b1 -> develop

### la pezza
per portare a manella dei changes in AWS
1. chiedere a riccardo
2. a