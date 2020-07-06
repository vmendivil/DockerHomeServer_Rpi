# Setting up 

1) Create "docker" folder
    - Run: mkdir ~/docker
    - Run: sudo setfacl -Rdm g:docker:rwx ~/docker
    - Run: sudo chmod -R 775 ~/docker
2) Clone repository with contents under folder created in previous step
    - Create valid version of all *.example files
    - Add users to ".htpasswd" (https://www.web2generators.com/apache-tools/htpasswd-generator)
        - If needed, use following command to generate user:password that is already escaped: echo $(htpasswd -nb username mystrongpassword) | sed -e s/\\$/\\$\\$/g
    - Set 600 permission to acme.json file, run: chmod 600 traefik2/acme/acme.json
3) Create network adapter
    - Run: docker network create t2_proxy
        - If you want to define ip addresses, run instead: docker network create --gateway 192.168.90.1 --subnet 192.168.90.0/24 t2_proxy
4) Configure Cloudflare
    - Create "A" record for your domain and your public ip address
    - Create "CNAME" for your subdomain, in this case, "traefik", your address will be: traefik.yourdomain.com
5) Run docker-compose definition
    - Run: docker-compose -f docker-compose-traefik2.yml up -d
    - See logs to validate all is correct
        - Run: docker logs -tf --tail="50" traefik
        - Or instead, open the logs from the container in docker (portainer UI)
6) Make a test
    - Browse for: traefik.yourdomain.com
    - Make sure http authentication is working
    - Make sure is running over https
    - Make sure there is no error in the logs
    - Make sure SSL certificates are valid
    - Validate you have data in traefik.log
    - Validate valid data on acme.json
7) Open "docker-compose-traefik2.yml" and comment all aplicable lines after first execution
    - Identify lines marked with initial commments using ##
    - Delete all content of "acme.json"

##### REFERENCES

Configuration
    Configuration guide: https://www.smarthomebeginner.com/traefik-2-docker-tutorial/#4_Proper_DNS_Records
    Cloudflare setup: https://www.smarthomebeginner.com/cloudflare-settings-for-traefik-docker/

Sample files: 
    .env: https://github.com/htpcBeginner/docker-traefik/blob/master/.env.example
    .bash_aliases: https://github.com/htpcBeginner/docker-traefik/blob/master/.bash_aliases.example
