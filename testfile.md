

### Commands 
- Stop all running containers
```
docker stop $(docker ps -q)
docker ps -q | xargs docker stop
```
- Remove all stopped containers
```
docker ps -a | grep "Exited" | awk '{print $1 }' | xargs docker rm
```
- Remove all `<none>` images
```
docker images | grep '<none>' | awk '{print $3 }' | xargs docker rmi
```

**NOTE:**

**If no any containers/images, will have problem when execute the front commands.**

**Also, make sure current user is in `sudoers` list. Else need add `sudo` in the front of each commands.**

