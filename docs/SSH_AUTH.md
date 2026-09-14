#SSH Authentication

1. Generate ssh key `ssh-keygen -t rsa -b 4096`  for accesing device
2. add public key into `authorized list` for receiving device
```
mkdir -p ~/.ssh
chmod 700 ~/.ssh
echo "key from ~/.ssh/id_rsa.pub" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```
