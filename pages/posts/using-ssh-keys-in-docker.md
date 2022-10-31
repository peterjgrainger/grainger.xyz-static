---
  
  title: Using private TLS/SSL keys in Docker
  date: 2017-08-31
---
  
## The Problem
At work I've been trying to consolidate similar functionality into modules to avoid repeating myself and give myself less work, the issue is that if you want to use an internal repo only accessible via a username and password or SSH key accessing repos gets either complicated or insecure so I looked for a solution

## Possible Solutions
1. Copy the key in and squash (CONS: not sure how I do this in a docker-compose file)http://blog.cloud66.com/pulling-git-into-a-docker-image-without-leaving-ssh-keys-behind/ \n
2. Copy the key in on the build step and add to image. (CONS: Not very secure :( ) 
3. Use the key as a build argument. (CONS: see 2) 
4. Dockerise something like https://www.vaultproject.io/ run that up first, add the key and use that within the node containers to get the latest key. (CONS: probably lots of work, maybe other issues?) 
5. Use Docker secrets and docker stack deploy and store the key in docker secrets (CON: docker stack deploy has no support for docker volumes yet. See here https://docs.docker.com/compose/bundles/#producing-a-bundle unsupported key 'volumes')

## An even better solution!
A new feature in Docker 17.05+!  Docker Multi stage builds!  https://docs.docker.com/engine/userguide/eng-image/multistage-build/ now I can copy the key in on the build step and it isn't stored in the image.  See this stack overflow question for more details: https://stackoverflow.com/questions/44756816/use-github-private-repo-deploy-key-inside-build-stage-in-docker-for-npm-install