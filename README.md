# ostree-dockerfile-builder

Build a Dockerfile into an OSTree repository.

## Usage

ostree-dockerfile-builder --repo=$OSTREE_REPO --tag=$TAG

The OCI configuration can be generated and stored under /exports if
ocitools is installed on the system.

## Example

```
# mkdir repo
# ostree --repo=repo --init=archive-z2
```

This is needed to fetch the Fedora image, that is used in the FROM
command later.  Remember that ocitools is needed by
the `--generate-oci-config` option.

```
# docker pull fedora
# ostree-dockerfile-builder --import-from-docker fedora
# cat > Dockerfile <<EOF
FROM fedora
RUN dnf install -y flannel && dnf clean all
CMD ["/usr/bin/flanneld", "-etcd-endpoints=http://127.0.0.1:2379"]
EOF

# ostree-dockerfile-builder --repo=repo --tag=flannel --generate-oci-config

```

## TODO

- Support more Docker commands, like `ADD`.
- Support generation of the configuration from a running container.
