# Dashy Installation

## Pull the latest image

Dashy recommends using a version tag, not just `latest`.

```bash
docker pull lissy93/dashy:2.1.1
```

## Run Dashy

```bash
docker run -d \
  --name dashy \
  -p 8080:80 \
  lissy93/dashy:2.1.1
```

## Access Dashy

Open your browser and navigate to:

```text
http://<my_ubuntu_vm_ip>:8080/
```

And you are all set.

Then configure the YAML file according to your preference of layout.
