---
title: Difference among docker IDs
date: 2019-12-18 14:36:51
tags: docker
categories:
- docker
---

## Summary

In this post, we will discuss diffrent tyeps of image id displayed in the result of `docker inspect ${image_id}`

All operations related to docker are based on `docker version > v1.10`

---

### Layer ID

`Layer` in RootFS is a fundamental mechanism of dockers, which enables `sharing the same basic image layer` in diffrent built images.

Each layer has its `parent layer` and `a unique id` generated by `hash digest algorithm` based on the content in this layer.

---

### Image Id

Before `docker v1.10`, the image id will simply be same with the id of the top layer. Same as parent field.

Since `docker v1.10`, images and layers are no longer synonymous. Layers are now identified by a digest, which takes the form `algorithm:hex`;

```
sha256:831c5620387fb9efec59fc82a42b948546c6be601e3ab34a87108ecf852aa15f
```

Layers no longer belong to images. They are merely collections of files and directories.

Right now, image id is now identified by a digest, which takes the form `algorithm:hex`. It's a computed SHA256 hash of the image configuration object which is the metadata of certain image.

---

## Distribution Id in Registry

```
docker push custom/cluster-manager:1.0.1
The push refers to a repository [docker.io/custom/cluster-manager]
f22bfascdf82: Pushed 
5f7aaf18da86: Layer already exists 
4d12ab9015d4: Pushed 
latest: digest: sha256:7f63e366abf377e2658e458ea1f6d5e002ff0cfd9ff28307hfc1b45aea2f820 size: 6128
```



When a image is pushed to a registry, docker daemon will `compress the image for network efficiency` and a `manifest` will also be created containing the contents of the image and digests of the compressed layer content.

The digest of a compressed image can be referred to as a `distribution digest`.

---

# References

```
https://windsock.io/explaining-docker-image-ids/
```