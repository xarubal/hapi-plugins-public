# Versión persoal de hapi-plugins-public

* Proxecto orixinal: [Hapi Plugins](https://github.com/hapijs-edge/hapi-plugins.com)
* Notas para a posta en marcha:
  * Corrixir o problema do schema de authenticacion (`MongoDB-CR Authentication failed`):
  ```
  $ docker-compose up -d mongodb
  $ docker-compose exec mongodb mongo
  > use admin
  > db.system.users.remove({})
  > db.system.version.remove({})
  > db.system.version.insert({ "_id" : "authSchema", "currentVersion" : 3 })
  > quit()
  $ docker-compose down mongodb
  $ docker-compose up -d mongodb
  ```
  * Crear usuario na base de datos de Mongodb con rol readWrite:
  ```
  $ docker-compose exec mongodb mongo
  > use hapi-plugins-public
  > db.createUser({ user: 'hapi', pwd: 'password12345', roles: [ { role: "readWrite", db: "hapi-plugins-public" } ] });
  ```
  * Redirixir mediante NAT os portos 8080 e 8088 a localhost.
  * Instalar os paquetes da aplicación localmente antes de arrincar:
  ```
  $ docker run -ti -v "$PWD"/app:/app -w /app node:4 npm install
  ```
  * A carga inicial de datos faise mediante a URL `/admin/plugins/populate/source`.

