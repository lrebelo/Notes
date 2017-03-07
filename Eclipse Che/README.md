# Eclipse Che notes


## Eclipse Che start command

_docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock -e CHE_HOST=IP_ADDRESS -v /home/user/cd:/data eclipse/che start_

## Eclipse Che stop command

_docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock -e CHE_HOST=IP_ADDRESS -v /home/user/cd:/data eclipse/che stop_


## Eclipse Che update commands

first run..

* _docker pull eclipse/che_

then run.. (this will upgrade and start che)

* _docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock -e CHE_HOST=IP_ADDRESS -v /home/user/cd:/data eclipse/che upgrade_


## Eclipse che install on debian 8

_I'll do it next time I do a fresh install..._


***

##### References..

```
https://eclipse.org/che/docs/setup/managing/index.html
```

```
https://eclipse.org/che/docs/setup/getting-started/index.html
```
