# Docker_WebServer-DB

4.
    a. Récupérer l'image sur le Docker Hub :
        -   `docker pull httpd`

    b. 
        -   `docker image ls`
            Retour : 
                REPOSITORY                           TAG       IMAGE ID       CREATED         SIZE
                httpd                                latest    b304753f3b6e   28 hours ago    145MB
    
    c.
        -   `<!doctype html>
            <html>
            <head>
                <title>This is the title of the webpage!</title>
            </head>
            <body>
                <p>This is an example paragraph. Anything in the <strong>body</strong> tag will appear on the page, just like this <strong>p</strong> tag and its contents.</p>
            </body>
            </html>`

    d.  
        -   `docker run -p 8080:80 --name webserver -v $(pwd)/src/index.html:/usr/local/apache2/htdocs/index.html -i -t httpd`
            Retour :
                AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
                    AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
                    [Wed Mar 08 11:09:48.263962 2023] [mpm_event:notice] [pid 1:tid 140538132909376] AH00489: Apache/2.4.55 (Unix) configured -- resuming normal operations
                    [Wed Mar 08 11:09:48.264162 2023] [core:notice] [pid 1:tid 140538132909376] AH00094: Command line: 'httpd -D FOREGROUND'
                    172.17.0.1 - - [08/Mar/2023:11:11:09 +0000] "GET / HTTP/1.1" 200 293
                    172.17.0.1 - - [08/Mar/2023:11:11:09 +0000] "GET /favicon.ico HTTP/1.1" 404 196
                    172.17.0.1 - - [08/Mar/2023:11:11:11 +0000] "GET /flutter_service_worker.js?v=null HTTP/1.1" 404 196
                    172.17.0.1 - - [08/Mar/2023:11:12:01 +0000] "-" 408 -

            I check by going on : http://localhost:8080

    e.
        -   `docker stop webserver
            docker rm webserver
            docker create --name webserver -p 8080:80 httpd
            docker cp src/index.html webserver:/usr/local/apache2/htdocs/index.html
            docker start webserver`
    
6.
    a. 
        -   Dockerfile :    `FROM httpd:2.4
                             COPY index.html /usr/local/apache2/htdocs/`
            Build : `docker build . -t webserver_image`

    b.  
        -   `docker run --name webserver -d -p 8080:80 webserver_image`
    
    c.
        -   La procédure 5 utilise une image existante pour démarrer un conteneur, la procédure 6 crée une nouvelle image personnalisée à partir d'un Dockerfile et utilise cette image pour démarrer un conteneur. L'avantage de la procédure 5 est que c'est plus rapide et plus facile, la 6 est customisable ce qui est pratique pour automatisé des actions (ex: copy), plus long a mettre en place mais on peut gagner du temps sur la durée.
    
    
        


    

