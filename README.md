# NEW REPO #

This repo is archived, new repo in following URL: https://github.com/vmendivil/traefik2

# DockerMediaServer

Some docker images have specific tag for ARM architecture since they are intended to run on a Raspberry Pi 4.

For detailed instructions visit the post of the original author, link in the bottom, in Acknowledgments and References.

## Configuration 

0) Make sure to change Open Media Vault to a different port orther than 8080.

1) Create "docker" folder
    - Validate media doesn't have a folder already created if using a previous instance
        - Run: cd srv/dev*
    - Run: mkdir ~/docker
    - Run: sudo setfacl -Rdm g:docker:rwx ~/docker
    - Run: sudo chmod -R 775 ~/docker

2) Clone repository with contents under folder created in previous step
    - Create valid version of all *.example files
    - Add users to ".htpasswd" (https://www.web2generators.com/apache-tools/htpasswd-generator)
        - If needed, use following command to generate user:password that is already escaped: echo $(htpasswd -nb username mystrongpassword) | sed -e s/\\$/\\$\\$/g
    - Set 600 permission to acme.json file, run: chmod 600 traefik2/acme/acme.json

3) Create network adapters
    - Run: docker network create t2_proxy --gateway 192.168.90.1 --subnet 192.168.90.0/24 
    - Run: docker network create myvpn

4) Configure Cloudflare
    - Create "A" record for your domain and your public ip address
    - Create "CNAME" for your subdomain, in this case, "traefik", your address will be: traefik.yourdomain.com
    - Create "CNAME" for all your additional subdomains

5) Configure router port forwarding
    - Setup port forwarding for port 80
        - External port: 80
        - Internal port: 80
        - IP: Rasperry IP
        - Protocol: TCP
    - Setup port forwarding for port 443
        - External port: 443
        - Internal port: 443
        - IP: Rasperry IP
        - Protocol: TCP

6) Run docker-compose definition
    - Validate all environment folder locations in yaml file are created
    - Validate all environment files are properly created, some of them:
        - Traefik:
            - Create final file based on *.example files
        - shared/.htpasswd
        - .env
        - Wordpress:
            - config/php.conf.uploads.ini
        - Check for additional files to be created on yml file, otherwise error will be displayed when deploying
    - Ensure folders requiring specific permissions are created
    - Before first time execution, uncomment lines with ##
    - Create containers, run: docker-compose -f docker-compose-traefik2.yml up -d
    - See logs to validate all is correct
        - Run: docker logs -tf --tail="50" traefik
        - Or instead, open the logs from the container in docker (portainer UI)
    - Comment back lines with ##

7) Test environment
    - Browse to: traefik.yourdomain.com
    - Make sure http authentication is working
    - Make sure is running over https
    - Make sure there is no error in the logs
    - Make sure SSL certificates are valid
    - Validate you have data in traefik.log
    - Validate valid data on acme.json

8) Open "docker-compose-traefik2.yml" and comment all aplicable lines after first execution
    - Identify lines marked with initial commments using ##
    - Delete all content of "acme.json"

## Referenes

- Configuration guide: https://www.smarthomebeginner.com/traefik-2-docker-tutorial/
- Cloudflare setup: https://www.smarthomebeginner.com/cloudflare-settings-for-traefik-docker/
- Google OAuth: https://www.smarthomebeginner.com/google-oauth-with-traefik-2-docker/

Sample files: 
- .env: https://github.com/htpcBeginner/docker-traefik/blob/master/.env.example
- .bash_aliases: https://github.com/htpcBeginner/docker-traefik/blob/master/.bash_aliases.example

## YML Files

- https://github.com/htpcBeginner/docker-traefik
- https://github.com/CVJoint/traefik2

## Acknowledgments and References

- https://www.smarthomebeginner.com/traefik-2-docker-tutorial/#4_Proper_DNS_Records
- https://github.com/htpcBeginner/docker-traefik
