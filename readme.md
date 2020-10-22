## Out of Reach docker server
* build with `docker-compose build`
* set the postgres user password:
  * `POSTGRES_PASSWORD` in `docker-compose.yml`
  * `-Door.database.password=` in `sfs2x-service.vmoptions
* run with `docker-compose up -d`
* 