## What does it do?

This playbook allows you to provision a DigitalOcean droplet to serve static Jekyll websites using Nginx with the press of a button!

All of your domains will be secured with SSL certificates which are automatically renewed with Certbot. And a cronjob running on the droplet will make sure your websites are kept up-to-date by pulling the repositories containing your Jekyll files every minute and rebuilding the websites whenever a change is detected in the hash of the last commit.

To use this playbook, first build your website locally using Jekyll and push these into a Github repository linked to a machine account (see instructions for this below).

## Creating the droplet

- Rename /vars/secrets.yml.example to /vars/secrets.yml
- Create a personal access token in your DigitalOcean account and save its value in /vars/secrets.yml under `digital_ocean_token:`
- In /vars/host_vars.yml: adjust the domain and subdomain variables to your main domain and subdomain names
- Run the digitalocean playbook to create the droplet: `ansible-playbook -i inventory.yml create_droplet.yml --ask-become-pass`
- Copy the IP-adres from the output under de hosts in inventory.yml
- Set your username for the droplet in /vars/host_vars.yml


## Setting up a machine user for the droplet.

- Create a second Github account and invite this account as a collaborator to your subdomain repositories. **Important: in order to prevent hackers from overwriting the repositories containing your subdomains, make sure collaborators are not allowed to commit to the main branch of those repositories.**
- On the droplet, create an SSH key for the new Github account by running `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`
- Add the public key to the Github machine user account.
- Save your public and private keys locally in /vars/secrets.yml


## Setting up the droplet

Run the initial_setup playbook to set up the droplet: `ansible-playbook -i inventory.yml initial_setup.yml --ssh-common-args="-o StrictHostKeyChecking=no"`
