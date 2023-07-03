# Setup a service running on a localhost port to be accessible via Nginx

1. Create an Nginx config file at `/etc/nginx/sites-available`.
   1. Typically, we use the naming convension of the domain/sub-domain. So e.g. call the file "odk-print.stats4sdtest.online" if that is the domain to be used.
2. Paste in the content from the reverse-proxy-template file.
   1. Change the port and the server_name to match the correct setup.
3. Setup the domain/sub-domain correctly to point to the server's IP address. (e.g., using Hover.com)
4. Check if LetsEncrypt is installed on the server - run `certbot`. If the command is recognised, great. Otherwise:
   2. (Instructions from https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal)
   1. `sudo snap install core; sudo snap refresh core;`
   3. `sudo snap install --classic certbot`
   4. `sudo ln -s /snap/bin/certbot /usr/bin/certbot`
5. Run certbox with the nginx flag to automatically detect and update your nginx configuration: `sudo certbot --nginx`
   1. You may be asked to select which domain/sub-domain to get a certificate for.
   2. The command will get the certificate, automatically update the nginx config file *and* setup a cron job to automatically renew the certificate every 90 days.

6. Done.

