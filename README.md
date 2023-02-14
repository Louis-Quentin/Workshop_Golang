# Workshop_Golang

# 1: Préparer un projet en Go
  - Télécharger Go: 
    - https://go.dev/dl/go1.20.linux-amd64.tar.gz
    
  - Installer Go:
    - rm -rf /usr/local/go && tar -C /usr/local -xzf go1.20.linux-amd64.tar.gz
    - export PATH=$PATH:/usr/local/go/bin
  
  - Vérifier l'installation:
    - go version

# 2: Print Hello World!
  - Créer son premier projet Go
    - ```go mod init go_ws```

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

      Container="{votre_container}"

      [ "$(docker ps -a | grep $Container)" ] && docker stop $Container
      [ "$(docker ps -a | grep $Container)" ] && docker rm $Container
      docker build -t {votre_image} ../../
      docker run -d -p 5432:5432 --name $Container {votre_image}
      sleep 2
      docker exec -it $Container bash -c "sh /tmp/entrypoint.sh"
      ```

# 4: Relier sa db au back
  - Créer un package database:
    - créer database.go -> ```database/database.go```
    - database.go -> ```
	type Database struct {
	*gorm.DB
	} ```
    - dans main.go ->
    ```
	var db database.Database
	err := db.Init_database()
	if err := nil {
		log.Fatal(err)
	}
	```
    - dans database.go, créer la méthode suivante: ```func (database *Database) Init_database() (err error) {...}```

# 5: Créer sa première route
  - Main.go -> Initialiser un objet gin
    - le faire run sur le port 8080
  - Main.go -> créer une fonction ```func apply_routes(r *gin.Engine, db *database.Database) {...}``` 
    * c'est ici que vous allez définir toutes les routes de votre API
    - définir la route ```GET "/"```qui renverra le message de votre choix avec un code 200 si tout s'est bien passé, autrement 400

# 6: Créer un Middleware
  - écrire la fonction -> ```func (db *database.Database) gin.HandlerFunc {...}```
    * le but de cette fonction est de créer un context contenant la db afin de le return et qu'il soit appliqué aux routes

# 7: Signup & Signin
  - créer une route: ```POST "/register"```
    * vous devrez ici read le body afin d'en extraire les credentials puis trouver un moyen de les stocker dans la db
  - créer une route: ```POST "/login"```
    - vérifier les informations contenues dans la db puis valider ou non la connection

# 7: BONUS: Authentification JWT
  - A vous de Jouer !
    * n'hésitez pas à poser des questions
