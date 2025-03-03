# increase-wordpress-file-upload-limit
# PHP Configuration for Large File Uploads

This guide provides instructions for configuring PHP to handle large file uploads within a Docker container.

## 1. Edit php.ini File

First, access the container shell:

```bash
docker exec -it <container_id> /bin/sh
```

If you haven't created a php.ini from the production template, run:

```bash
cp /usr/local/etc/php/php.ini-production /usr/local/etc/php/php.ini
```

Install a text editor if not available:

```bash
# Update package list
apt-get update

# Install vim (or nano)
apt-get install vim
# OR
apt-get install nano
```

Edit php.ini:

### Using Vim:

```bash
vim /usr/local/etc/php/php.ini
```

Press `i` to enter insert mode. Find and update these lines:

```ini
upload_max_filesize = 6400M
post_max_size = 6400M
memory_limit = 12800M
max_execution_time = 300
max_input_time = 300
```

Press `Esc`, then type `:wq` to save and exit.

### Using Nano:

```bash
nano /usr/local/etc/php/php.ini
```

After making changes, press `Ctrl+O` to save and `Ctrl+X` to exit.

## 2. Check for Overriding Settings

Check if any additional configuration files in `/usr/local/etc/php/conf.d/` might be overriding these settings. If found, update or remove those files as necessary.

## 3. Restart the Container/Service

Apply changes by restarting the service inside the container:

```bash
service apache2 restart
# OR
service php-fpm restart
```

Or simply restart the entire container:

```bash
docker restart <container_id>
```

## 4. Verify Changes

Run these commands inside the container to verify the updated settings:

```bash
php -i | grep upload_max_filesize
php -i | grep post_max_size
php -i | grep memory_limit
```

You should see the updated values (6400M, 6400M, 12800M) in the output.
