# Docker_WebServer-DB

### Benoît JAUSSERAN
### git : https://github.com/bjausseran/Docker_WebServer-DB


## 4 
### a.
Récupérer l'image sur le Docker Hub :

- `docker pull httpd`

### b.
- `docker image ls`

Retour :

	REPOSITORY TAG IMAGE ID CREATED SIZE

	httpd latest b304753f3b6e 28 hours ago 145MB
### c.

	<!doctype  html>
	<html>
		<head>
			<title>This is the title of the webpage!</title>
		</head>
		<body>
			<p>This is an example paragraph. Anything in the <strong>body</strong> tag will appear on the page, just like this <strong>p</strong> tag and its contents.</p>
		</body>
	</html>
### d.

- `docker run -p 8080:80 --name webserver -v $(pwd)/src/index.html:/usr/local/apache2/htdocs/index.html -i -t httpd`

Retour :

	AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
	AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
	[Wed Mar 08 11:09:48.263962 2023] [mpm_event:notice] [pid 1:tid 140538132909376] AH00489: Apache/2.4.55 (Unix) configured -- resuming normal operations
	[Wed Mar 08 11:09:48.264162 2023] [core:notice] [pid 1:tid 140538132909376] AH00094: Command line: 'httpd -D FOREGROUND'
	172.17.0.1 - - [08/Mar/2023:11:11:09 +0000] "GET / HTTP/1.1" 200 293
	172.17.0.1 - - [08/Mar/2023:11:11:09 +0000] "GET /favicon.ico HTTP/1.1" 404 196
	172.17.0.1 - - [08/Mar/2023:11:11:11 +0000] "GET /flutter_service_worker.js?v=null HTTP/1.1" 404 196
	172.17.0.1 - - [08/Mar/2023:11:12:01 +0000] "-" 408 - `

  

Je vérifier ici : `http://localhost:8080`

### e.

	docker stop webserver
	docker rm webserver
	docker create --name webserver -p 8080:80 httpd
	docker cp src/index.html 	webserver:/usr/local/apache2/htdocs/index.html
	docker start webserver`

### 6
### a.
**Dockerfile** : 

	FROM httpd:2.4
	COPY index.html /usr/local/apache2/htdocs/`

**Build** : 

	docker build . -t webserver_image

### b.
	docker run --name webserver -d -p 8080:80 webserver_image

### c.
   La procédure 5 utilise une image existante pour démarrer un conteneur, la procédure 6 crée une nouvelle image personnalisée à partir d'un Dockerfile et utilise cette image pour démarrer un conteneur. L'avantage de la procédure 5 est que c'est plus rapide et plus facile, la 6 est customisable ce qui est pratique pour automatisé des actions (ex: copy), plus long a mettre en place mais on peut gagner du temps sur la durée.

## 7
### a.
	docker pull mysql:5.7
	docker pull phpmyadmin/phpmyadmin
	
### b.
**MySQL** :
	
	docker run -d --name mysqldb -e MYSQL_ROOT_PASSWORD=password mysql:5.7
	
- Le server MySQL ne s'était pas lancé automatiquement, j'ai fait : `mysql -u root -p` dans le container.

**PhpMyAdmin**

	docker run -d --name phpmyadmin --link mysqldb:db -p 8080:80 phpmyadmin/phpmyadmin

On se connecte avec root;password sur localhost:8080 et on crée une table comme à l'accoutumé.

## 8
On crée un docker-compose.yml basique :

```
version: '3'
 
services:
	mysqldb:
	image: mysql:5.7
	restart: always
	environment:
		MYSQL_ROOT_PASSWORD: root
		MYSQL_DATABASE: database
phpmyadmin:
	image: phpmyadmin/phpmyadmin
	restart: always
	ports:
		- 8080:80
	environment:
		PMA_HOST: mysqldb
```
  

Puis : 

	docker compose build
	docker compose up
### a.
Cela permet de monter plusieurs containers d'un coup sans avoir à taper une série de commandes avec beaucoup d'options.

### b.
on rajoute 
		
	environment:
		MYSQL_ROOT_PASSWORD: password
		MYSQL_USER: user
		MYSQL_PASSWORD: password
		MYSQL_DATABASE: database

## 9.On crée un docker-compose.yml :

```
version: '3'

services:
	web:
		image: praqma/network-multitool
		container_name: web
		command: sleep infinity
		networks:
			- frontend
			- backend

	app:
		image: praqma/network-multitool
		container_name: app
		command: sleep infinity
		networks:
			- backend

	db:
		image: praqma/network-multitool
		container_name: db
		command: sleep
		networks:
			- frontend

networks:
	frontend:
	backend:
```

Puis : 

	docker compose build
	docker compose up

### a.
A l'aide de ces commandes :
	
	docker inspect {containerId or name}
Retour pour web :
```
[
    {
        "Id": "ef30eb641e0790e36ea7e12c118d7a8f3923875b0c566793863e4af5f114f92d",
        "Created": "2023-03-08T14:24:25.4046045Z",
        
        {...}
        
            "Networks": {
                "docker_webserver-db_backend": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": [
                        "web",
                        "web",
                        "ef30eb641e07"
                    ],
                    "NetworkID": "101e107e7d1616e5ad1f1a34d8bb0d4d8f95819f1be1b4815b481b37306e9e83",
                    "EndpointID": "5194af06f60edc5d65767c243acfe67aa2a40459ec0fe4ef3692da797af4c293",
                    "Gateway": "172.23.0.1",
                    "IPAddress": "172.23.0.3",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:17:00:03",
                    "DriverOpts": null
                },
                "docker_webserver-db_frontend": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": [
                        "web",
                        "web",
                        "ef30eb641e07"
                    ],
                    "NetworkID": "ff6e5255d44cb5995ead33c4f0b6cf677c3f667170c91a5c58fa40a05dbe5c64",
                    "EndpointID": "170266cbf054e56b2cbf2947c61eefc3217661adf484315e7a084bf35566a1c8",
                    "Gateway": "172.22.0.1",
                    "IPAddress": "172.22.0.3",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:16:00:03",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

On regarde la ou les lignes Gateway ou bien IpAddress, ou plus directement on peut faire :

	docker inspect -f '{{range .NetworkSettings.Networks}}{{.Gateway}}, {{end}}' db

Retour : `172.22.0.1,`

	docker inspect -f '{{range .NetworkSettings.Networks}}{{.Gateway}}, {{end}}' app

Retour : `172.23.0.1,`

	docker inspect -f '{{range .NetworkSettings.Networks}}{{.Gateway}}, {{end}}' web

Retour : `172.23.0.1, 172.22.0.1,`

  

On voit que db n'est pas sur le même reseau que app, et que web est sur les deux.

### c.
Cela permet de limiter la connexion entre les différents services, ce qui permet de renforcer la sécurité et d'isoler les services. Isoler les services permets d'en maintenir debout lorsque une partie de l'infra tombe.
