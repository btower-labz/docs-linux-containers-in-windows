https://docs.fedoraproject.org/en-US/fedora-coreos/producing-ign/
docker pull pull quay.io/coreos/fcct:release
docker run -i --rm quay.io/coreos/fcct:release --pretty --strict < main.yaml > main.ign

name=opt/com.coreos/config,file=${IGNITION_CONFIG}


```yaml
variant: fcos
version: 1.0.0
passwd:
  users:
    - name: core
      ssh_authorized_keys:
        - ecdsa-sha2-nistp521 AAAA...
```

```json
{
  "ignition": {
    "version": "3.0.0"
  },
  "passwd": {
    "users": [
      {
        "name": "core",
        "sshAuthorizedKeys": [
          "ecdsa-sha2-nistp521 AAAAE2V."
        ]
      }
    ]
  }
}
```
