## 0. Run Local

```
************** Rodar SQL ************** 
docker run --name sqlserver --network mynetwork -e ACCEPT_EULA=Y -e MSSQL_PID="Developer" -e MSSQL_SA_PASSWORD="123abc!@#" -e MSSQL_TCP_PORT=1433 -p 1433:1433 -d mcr.microsoft.com/mssql/server:2017-latest

************** Validar SQL ************** 
docker exec -it sqlserver /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "123abc!@#" -Q "select * from sys.databases"
************** Criar DATABASE SQL ************** 
docker exec -it sqlserver /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "123abc!@#" -Q "CREATE DATABASE mydrivingDB;"


************** LOAD BANCO DE DADOS ************** 
docker run --name dataload --network mynetwork -e SQLFQDN=sqlserver -e SQLUSER=SA -e SQLPASS="123abc!@#" -e SQLDB=mydrivingDB -d registryltb1998.azurecr.io/dataload:1.0

************** Rodar APP ************** 
docker run  --name poi --network mynetwork -d -p 8080:80 -e SQL_USER="SA" -e SQL_PASSWORD="123abc!@#" -e SQL_SERVER="sqlserver" -e SQL_DBNAME="mydrivingDB" -e ASPNETCORE_ENVIRONMENT="Local" poi:latest
```

## 1. Move Dockerfiles para Aplicações:
```
mv dockerfiles/Dockerfile_0 src/user-java/Dockerfile
mv dockerfiles/Dockerfile_1 src/tripviewer/Dockerfile
mv dockerfiles/Dockerfile_2 src/userprofile/Dockerfile
mv dockerfiles/Dockerfile_3 src/poi/Dockerfile
mv dockerfiles/Dockerfile_4 src/trips/Dockerfile
```

## 2. Build Image Apps:
```
cd src\trips
docker build -t registryltb1998/trips:latest .


cd src\user-java
docker build -t registryltb1998/user-java:latest .

cd src\userprofile
docker build -t registryltb1998/userprofile:latest .

cd src\tripviewer
docker build -t registryltb1998/tripviewer:latest .

cd src\poi
docker build -t registryltb1998/poi:latest .
```

## 3. Push ACR
```
az login

az account set --subscription "Azure subscription 1"

az acr login -n registryltb1998

docker push registryltb1998.azurecr.io/trips:latest
docker push registryltb1998.azurecr.io/user-java:latest
docker push registryltb1998.azurecr.io/userprofile:latest
docker push registryltb1998.azurecr.io/tripviewer:latest
docker push registryltb1998.azurecr.io/poi:latest
```

### Check Publish
```
C:\Users\Bolsa Balcão Brasil>az acr repository show -n registryltb1998 --repository poi -o table
CreatedTime                   ImageName    LastUpdateTime               ManifestCount    Registry                    TagCount
----------------------------  -----------  ---------------------------  ---------------  --------------------------  ----------
2022-09-13T14:45:37.2706574Z  poi          2022-09-13T14:45:37.361834Z  1                registryltb1998.azurecr.io  1

C:\Users\Bolsa Balcão Brasil>az acr repository show -n registryltb1998 --repository trips -o table
CreatedTime                   ImageName    LastUpdateTime                ManifestCount    Registry                    TagCount
----------------------------  -----------  ----------------------------  ---------------  --------------------------  ----------
2022-09-13T14:46:01.7457862Z  trips        2022-09-13T14:46:01.8217954Z  1                registryltb1998.azurecr.io  1

C:\Users\Bolsa Balcão Brasil>az acr repository show -n registryltb1998 --repository tripviewer -o table
CreatedTime                   ImageName    LastUpdateTime                ManifestCount    Registry                    TagCount
----------------------------  -----------  ----------------------------  ---------------  --------------------------  ----------
2022-09-13T14:46:41.1787247Z  tripviewer   2022-09-13T14:46:41.2852726Z  1                registryltb1998.azurecr.io  1

C:\Users\Bolsa Balcão Brasil>az acr repository show -n registryltb1998 --repository user-java -o table
CreatedTime                   ImageName    LastUpdateTime                ManifestCount    Registry                    TagCount
----------------------------  -----------  ----------------------------  ---------------  --------------------------  ----------
2022-09-13T14:47:30.6279305Z  user-java    2022-09-13T14:47:30.7275177Z  1                registryltb1998.azurecr.io  1

C:\Users\Bolsa Balcão Brasil>az acr repository show -n registryltb1998 --repository userprofile -o table
CreatedTime                   ImageName    LastUpdateTime                ManifestCount    Registry                    TagCount
----------------------------  -----------  ----------------------------  ---------------  --------------------------  ----------
2022-09-13T14:48:49.4363881Z  userprofile  2022-09-13T14:48:49.5144395Z  1                registryltb1998.azurecr.io  1
```