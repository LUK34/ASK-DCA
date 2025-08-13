- This command will forcefully delete all Docker images.
```
 docker rmi -f $(docker images -q)
```

- This command will forcefully delete a specific docker image.
```
 docker rmi -f <image_id_or_name>
```