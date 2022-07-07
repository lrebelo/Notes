# Bitwarden / Vaultwarden Notes

> Notes on docker deployment

## Docker command

```
docker pull vaultwarden/server:latest
docker run -d --name vaultwarden -v /mnt/vw-data/:/data/ -p 9078:80 vaultwarden/server:latest
```

Do the nginx certbot or vaultwarden won't work.

## Block public signups

Add to docker command:

`-e SIGNUPS_ALLOWED=false`

## Activate Admin panel

Add to docker command:

`-e ADMIN_TOKEN=secret0secret1secret2secret3secret4`


### Sources
[](https://mariushosting.com/page/2/?s=bitwarden)