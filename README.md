# Autobahn
Autobahn is the Ueberholspur for your Composer-based WordPress stack. German Engineering

## Usage
Start a new project by running:
```
composer create-project mrgrain/autobahn
```

Set up your local environment by copying `.env.example` to `.env` and edit it:
```
cp .env.example .env
vim .env #change .env to your needs
```

...and change `wp-cli.yml` to your needs (you should leave `path` as it is):
```
vim wp-cli.yml #change wp-cli.yml to your needs
```

Install Wordpress via WPCLI:
```
./vendor/bin/wp core install
```
