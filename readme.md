# WordPress Handy How Tos

## How To Migrate WordPress Site

### Step 1: Tar or Zip all Website Files
- login via SSH or FTP and zip the entire contents of root wordpress installation
- NOTE: If you .zip your files, you'll *have to update file and folder permissions* following restoration. If you .tar your files and folders, permissions will be preserved.
- If you are using an existing backup plugin, that means you'll likely have a .zip archive
- If you manually are archiving your files, use this command to tar them:
```
$tar -cvf  /home/user_dir.tar .  
# replace path with your own: tar -cvf {{path-to-save-tar-file}} {{what-to-tar}} 
```
- I find it easiest to SSH in and just tar up the root directory
- Download the archive to your local environment for just in case
- NOTE: If you are using a backup plugin, and have multiple archives available, now might be a good time to download as many as you want. Generally the backups do not contain themselves the backups (so you are only getting a 1x only snapshot of the site). If you are nuking your old data, you will lose all the backups that you did not download (perhaps you only downloaded the one you wanted to restore). Just to prevent an anxiety attack, should something happent to that single zip file, grab a few. Or grab a whole month, so you know at least you have a month of archives to fall back on should there be any issues with your generated zip or tar.
- An even better solution is prior to nuking your existing data: try setting up the site on a throw away sub domain or folder on your current host: see if you can get your restored backup working, and once confirmed, can safely delete your old data.

### Step 2: Backup database
- using phpMyAdmin or any other preferred tool, "Export" your database as a .sql file
- in phpMyAdmin, be sure to tick all tables, then click "Export". Lookup tutorials for this process if needed.

### Step 3: Prepare new host
- Now that you have backed up your files and database, you need to prepare for your new host
- If you are restoring an existing installation, you may want to create a new folder called `__OLD` and put **ALL** folder contents of wordpress installation into there, as we will be downloading our tar and expanding it
- If you're using a new host, setup some web space where you want your wordpress installation

### Step 4: Setup new database
- If you are restoring on your current host, use phpMyAdmin or another tool to rename your database: `{{db-name}}__OLD`.
- Create a new database with the same name as your old one (minus the `__OLD`)
- Add the same user that had privileges on your old database to your new one
- Be sure this user has ALL PERMISSIONS granted
- Create your new empty database with the same name as the old one
- Now "Import" your backed up database .sql file
- Your new database should show all the same tables following import

### Step 5: Restore backed up files
- SSH in to where you want your application 
- If restoring, this is the root folder where your WP installation was
- I generally upload my .tar file somewhere and then wget it down to new server:
```
wget {{path-to-your-tar-or-zip-file}}
```
- NOTE: You can also use FTP to login and expand an archive, but warning, this is going to take you FOREVER. SSH using wget and tar files will be like a blazing fast train.
- Expand your tar archive via:
```
$tar -xvf user_dir.tar
# tar -xvf {{name-of-tar-file}}
```
- NOTE: If you expanded a zip (e.g, `unzip backup.zip`) you need to set **File Permissions** to: 644 and **Folder Permissions** to 755. Checkout [this resource](https://www.wpbeginner.com/beginners-guide/how-to-fix-file-and-folder-permissions-error-in-wordpress/) or another to see how to do this Filezila or over SSH. If you tar'd your files, your permissions were preserved.

### Step 6: Edit wp-config.php
- Once you have uploaded your backed up database and your backed up files, you may need to edit wp-config:
  - if you changed site name, you'll have to update your site name in your wp-config: see [how to change site name](https://wordpress.org/support/article/changing-the-site-url/#edit-wp-config-php) for more info.
  - it is generally recommended to change the salts as well, simply change a few characters in each string
- save changes to wp-config

### Step 7: Your site should work!
- clean up by deleting any downloaded archived zips or tar files (don't just leave these on your server)

## Enable Automatic Updates
- in `wp-config.php` add the following:
```
define( ‘WP_AUTO_UPDATE_CORE’, true );
```
