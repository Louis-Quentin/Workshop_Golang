# Workshop_Golang

# 1: Préparer un projet en Go
  - Télécharger Go: 
    - https://go.dev/dl/go1.20.linux-amd64.tar.gz
    
  - Installer Go:
    - rm -rf /usr/local/go && tar -C /usr/local -xzf go1.20.linux-amd64.tar.gz
    - export PATH=$PATH:/usr/local/go/bin
  
  - Vérifier l'installation:
    - go version

# 2: Créer sa première route
  - Créer son premier projet Go
    - go mod init go_ws

  - Télécharger le framework Gin:
    - go get -u github.com/gin-gonic/gin

  - Hello World !
    - import ```import "github.com/gin-gonic/gin"```
    - à vous de trouver comment print "Hello World !"
    
  - Tester son projet:
    - go run main.go 

# 3: Initialiser une db postgres
  - Créer la db postgres:
    - créer un fichier entrypoint.sh avec ce contenu:
      ```
      #!/bin/bash
      set -e

      psql -v ON_ERROR_STOP=1 --username "$POSTGRES_USER" --dbname "$POSTGRES_DB" <<-EOSQL
	    CREATE USER docker;
	    CREATE DATABASE docker;
	    GRANT ALL PRIVILEGES ON DATABASE docker TO docker;
      ```
    - créer un fichier run_db.sh avec ce contenu:
      ```
      set -e

      Container="clothway_postgres"

      [ "$(docker ps -a | grep $Container)" ] && docker stop $Container
      [ "$(docker ps -a | grep $Container)" ] && docker rm $Container
      docker build -t clothway_db ../../
      docker run -d -p 5432:5432 --name $Container clothway_db
      sleep 2
      docker exec -it $Container bash -c "sh /tmp/entrypoint.sh"
      ```


# 4: Relier sa db au back

# 5: Signup & Signin

# 6: BONUS: Authentification JWT
