# Docker_WebServer-DB

4.
    a. Récupérer l'image sur le Docker Hub :
        -   docker pull httpd

    b. 
        -   docker image ls
            Retour : 
                REPOSITORY                           TAG       IMAGE ID       CREATED         SIZE
                httpd                                latest    b304753f3b6e   28 hours ago    145MB
    
    c.
        -   <!doctype html>
            <html>
            <head>
                <title>This is the title of the webpage!</title>
            </head>
            <body>
                <p>This is an example paragraph. Anything in the <strong>body</strong> tag will appear on the page, just like this <strong>p</strong> tag and its contents.</p>
            </body>
            </html>

    d.  
        -   docker run -v /src:/src -w /src -i -t httpd
            Retour :
                AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
                AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.2. Set the 'ServerName' directive globally to suppress this message
                [Thu Mar 02 11:22:29.849846 2023] [mpm_event:notice] [pid 1:tid 140155531181376] AH00489: Apache/2.4.55 (Unix) configured -- resuming normal operations
                [Thu Mar 02 11:22:29.849973 2023] [core:notice] [pid 1:tid 140155531181376] AH00094: Command line: 'httpd -D FOREGROUND'
