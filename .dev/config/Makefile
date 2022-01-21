#!/usr/bin/make

CURRENT_USER := "CURRENT_USER"
OS := $(shell uname)

## Shell confirming
confirm:
	@echo -n "Are you sure? [y/N] " && read ans && [ $${ans:-N} = y ]

## Open Chrome with disable-web-security
open-chrome:
ifeq ($(OS),Darwin)
	"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" "http://URL.local:3001/it_it" --disable-web-security --user-data-dir=tmp/chromeTemp
	rm -rf tmp/*
else ifeq ($(OS),Linux)
	sudo -u $(CURRENT_USER) "/usr/bin/google-chrome-stable" "http://URL.local:3001/it_it" --disable-web-security --user-data-dir=tmp/chromeTemp
	rm -rf tmp/*
else
	"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" "http://URL.local:3001/it_it" --disable-web-security --user-data-dir=tmp/chromeTemp
	rm -rf tmp/*
endif

## Up the Docker Containers
up: starting open-chrome

starting:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml up -d
else
	docker-compose up -d
endif

## Up with verbose mode the Docker Containers
up-verbose:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml up
else
	docker-compose up
endif

## Up with force build the Docker Containers
up-build:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml up --build
else
	docker-compose up --build
endif

## Stop the Docker Containers
stop:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml stop
else
	docker-compose stop
endif

## Down the Docker Containers
down:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml down
else
	docker-compose down
endif

## Kill the Docker Containers
kill:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml kill
else
	docker-compose kill
endif

## Shell fix-permission commands into container
fix-permission:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml exec -u root apache bash -c "find /var/www/html/ -type d -exec chmod 777 {} \; ; find /var/www/html/ -type f -exec chmod 777 {} \; ; chown -R :root /var/www/html/ ;"
else ifeq ($(OS),Linux)
	docker-compose exec -u root apache bash -c "find /var/www/html/ -type d -exec chmod 777 {} \; ; find /var/www/html/ -type f -exec chmod 777 {} \; ; chown -R :root /var/www/html/ ;"
else
	winpty docker-compose exec -u root apache bash -c "find /var/www/html/ -type d -exec chmod 777 {} \; ; find /var/www/html/ -type f -exec chmod 777 {} \; ; chown -R :root /var/www/html/ ;"
endif

## Execute the container Apache
exec-apache:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml exec --user root apache bash
else ifeq ($(OS),Linux)
	docker-compose exec --user root apache bash
else
	winpty docker-compose exec --user root apache bash
endif

## Execute the container DB
exec-db:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml exec --user root db sh
else ifeq ($(OS),Linux)
	docker-compose exec --user root db sh
else
	winpty docker-compose exec --user root db sh
endif

## Execute shell Magento commands into container
exec-magento:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml exec --user root apache bash -c "cd /var/www/html/bin/ ; php -d memory_limit=-1 magento $(CMD) ;"
else ifeq ($(OS),Linux)
	docker-compose exec -u root apache bash -c "cd /var/www/html/bin/ ; php -d memory_limit=-1 magento $(CMD) ;"
else
	winpty docker-compose exec -u root apache bash -c "cd /var/www/html/bin/ ; php -d memory_limit=-1 magento $(CMD) ;"
endif

## Execute shell n98 commands into container
exec-n98:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml exec --user root apache bash -c "cd /var/www/html/ ; php -d memory_limit=-1 ./vendor/n98/magerun2/bin/n98-magerun2 $(CMD) ;"
else ifeq ($(OS),Linux)
	docker-compose exec -u root apache bash -c "cd /var/www/html/ ; php -d memory_limit=-1 ./vendor/n98/magerun2/bin/n98-magerun2 $(CMD) ;"
else
	winpty docker-compose exec -u root apache bash -c "cd /var/www/html/ ; php -d memory_limit=-1 ./vendor/n98/magerun2/bin/n98-magerun2 $(CMD) ;"
endif

## Execute the container ElasticSearch
exec-elasticsearch:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml exec --user root elasticsearch sh
else ifeq ($(OS),Linux)
	docker-compose exec --user root elasticsearch sh
else
	winpty docker-compose exec --user root elasticsearch sh
endif

## Execute the container Node
exec-node:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml exec --user root node sh
else ifeq ($(OS),Linux)
	docker-compose exec --user root node sh
else
	winpty docker-compose exec --user root node sh
endif

## Execute the container Redis
exec-redis:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml exec --user root redis sh
else ifeq ($(OS),Linux)
	docker-compose exec --user root redis sh
else
	winpty docker-compose exec --user root redis sh
endif

## Execute the container Rabbitmq
exec-rabbitmq:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml exec --user root rabbitmq sh
else ifeq ($(OS),Linux)
	docker-compose exec --user root rabbitmq sh
else
	winpty docker-compose exec --user root rabbitmq sh
endif

## First setup
setup-first: confirm starting fix-permission installation

installation:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml exec -u root apache bash -c "cd /var/www/html/ ; php -d memory_limit=-1 /usr/local/bin/composer1 i ; chmod u+x /var/www/html/bin/magento ;"
else ifeq ($(OS),Linux)
	docker-compose exec -u root apache bash -c "cd /var/www/html/ ;  php -d memory_limit=-1 /usr/local/bin/composer1 i ; chmod u+x /var/www/html/bin/magento ;"
else
	winpty docker-compose exec -u root apache bash -c "cd /var/www/html/ ;  php -d memory_limit=-1 /usr/local/bin/composer1 i ; chmod u+x /var/www/html/bin/magento ;"
endif
	@echo '************ The setup first was successful ************'

## Upgrade setup
setup-upgrade: confirm starting updating

updating:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml exec -u root apache bash -c "cd /var/www/html/ ; php -d memory_limit=-1 /usr/local/bin/composer1 up --lock ;"
else ifeq ($(OS),Linux)
	docker-compose exec -u root apache bash -c "cd /var/www/html/ ;  php -d memory_limit=-1 /usr/local/bin/composer1 up --lock ;"
else
	winpty docker-compose exec -u root apache bash -c "cd /var/www/html/ ; php -d memory_limit=-1 /usr/local/bin/composer1 up --lock ;"
endif
	@echo '************ The setup upgrade was successful ************'

## Install n98 commands into container
install-n98:
ifeq ($(OS),Darwin)
	docker-compose -f docker-compose.mac.override.yml exec --user root apache bash -c "cd /var/www/html/ ; php -d memory_limit=-1 /usr/local/bin/composer1 require n98/magerun2 ;"
else ifeq ($(OS),Linux)
	docker-compose exec -u root apache bash -c "cd /var/www/html/ ; php -d memory_limit=-1 /usr/local/bin/composer1 require n98/magerun2 ;"
else
	winpty docker-compose exec -u root apache bash -c "cd /var/www/html/ ; php -d memory_limit=-1 /usr/local/bin/composer1 require n98/magerun2 ;"
endif