# Install CFEngine

```bash
export IGNORE_VERSION_CHECK=1
wget -q -O /etc/apt/trusted.gpg.d/cfengine.asc https://cfengine-package-repos.s3.amazonaws.com/pub/gpg.key
echo "deb https://cfengine-package-repos.s3.amazonaws.com/pub/apt/packages stable main" > /etc/apt/sources.list.d/cfengine-community.list
apt-get update
apt-get install cfengine-community
cf-agent --bootstrap policy_server_ip
```
