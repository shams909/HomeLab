#Dashy Installation

`Pull the latest image`

docker pull lissy93/dashy:2.1.1  `Dashy recommends using a version tag, not just latest. SO I grabbed the current stable release (2.1.1)`

Now,run Dashy

docker run -d \
  --name dashy \
  -p 8080:80 \
  lissy93/dashy:2.1.1

Now open in your browser:

http://<my_ubuntu_vm_ip>:8080/

And u r all set.

Then set the yaml file according to ur preference of layout.


