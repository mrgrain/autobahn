# Autobahn
Autobahn is the Ueberholspur for your Composer-based WordPress stack. German Engineering

## Usage
Start a new project by running:
```
composer create-project mrgrain/autobahn
```

Then set up your local environment:
```
cp .env.example .env
vim .env #edit .env to your needs
```
Install Wordpress via WPCLI:
```
./vendor/bin/wp core install --url="http://example.com" --title="Autobahn" --admin_user="admin" --admin_password="password" --admin_email="mail@example.com"
```
