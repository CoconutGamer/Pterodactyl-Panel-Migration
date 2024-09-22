# Pterodactyl Panel Migration: A Complete Guide
Migrating a Pterodactyl panel from one machine to another can be a daunting task, but with the right steps, it becomes a smooth and straightforward process. This guide will walk you through migrating both the Pterodactyl panel and Wings to a new server. We’ll cover how to back up, transfer, and configure everything to ensure a successful migration.You can also Watch the Video on Here. [YouTube](https://www.youtube.com/watch?v=7HYf7b8vagY)

By following this step-by-step guide, you'll have your panel up and running on your new server in no time.

## Prerequisites
Before starting the migration process, ensure the following:

- You have root access to both the old and new machines.
- You have MySQL installed on both servers.
- Both the old and new servers have the required dependencies for running [Pterodactyl](https://pterodactyl.io/).
- The new server meets the Pterodactyl system requirements.

# Step 1: Backup the Pterodactyl Panel
- To migrate your panel, start by backing up essential components on the old machine.

## 1.1 Backup the .env file
The .env file located in `/var/www/pterodactyl` contains sensitive environment variables like your APP_KEY. This file is critical for decrypting data and must be transferred to the new server.

```bash
cd /var/www/pterodactyl
```
Make sure the backup location is secure since this file holds sensitive information.

## 1.2 Backup the Database
Next, you need to export the Pterodactyl database. In this example, the database is named panel.

```bash
mysqldump -u root -p --opt panel > /var/www/pterodactyl/panel.sql
```

This command will export the entire panel database into a file named panel.sql inside ``/var/www/pterodactyl/``. You’ll need to transfer this file to your new server later.


# Step 2: Set Up Pterodactyl on the New Server
Before you can restore your data on the new server, you need to install the Pterodactyl panel.

## 2.1 Install the Pterodactyl Panel
Follow the official installation guide to set up the panel on your new server. This guide covers all the necessary steps, including installing the required dependencies, database setup, and configuring the web server.

## Step 3: Migrate the Database
Once the Pterodactyl panel is set up on your new machine, it’s time to restore your previous database.

### 3.1 Transfer the panel.sql file
Use scp or any other file transfer method to move the panel.sql file from the old server to the new one.

### 3.2 Import the Database
After transferring the SQL file, you can import the database into the new server’s MySQL instance.

```bash
mysql -u root -p panel < /var/www/pterodactyl/panel.sql
```

- Ensure that you are in the correct directory and your database name is correctly set.

# Step 4: Restore the .env File
Transfer the .env file from the old server to your new server and place it in ``/var/www/pterodactyl``.

Make sure you place the .env file in the correct location, as it is crucial for panel functionality.

# Step 5: Migrating Wings
Now that the panel is running on the new server, it’s time to set up and migrate Wings.

## 5.1 Install Wings
Follow the Wings installation guide to install Wings on your new machine. Ensure that the new Wings are properly configured to communicate with the Pterodactyl panel on the new server.

## 5.2 Transfer Volumes
Your game server data is stored in volumes, which need to be transferred to the new Wings machine. The default location for these volumes is ``/var/lib/pterodactyl/volumes/``.

Use the following command to copy the volumes from the old machine to the new one:


# Step 6: Update Allocations
Since the IP address of your new machine is likely different from the old one, you need to update the IP allocations in the database.

## 6.1 Get the New IP Address
Run the following command on your new Wings machine to find your new IP address:

```bash
hostname -I | awk '{print $1}'
```

## 6.2 Update the Database with the New IP
Log in to your MySQL database and update the allocations table with your new IP address. Replace newiphere with your new IP and oldiphere with your old IP.

```bash
mysql -u root -p
USE panel;
UPDATE allocations SET ip = 'newiphere' WHERE ip = 'oldiphere';
exit
```
# Conclusion
Congratulations! You've successfully migrated your Pterodactyl panel and Wings from one machine to another. This guide covered every step, from backing up important files to updating allocations on your new server. If you follow this procedure carefully, your new setup should run smoothly without any data loss.

By using this in-depth guide, you ensure that the migration is done with minimal downtime and that your users have a seamless experience on the new server.
