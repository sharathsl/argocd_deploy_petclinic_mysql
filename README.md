# Run below steps to deploy petclinic app integrated with mysql db using Argocd, provided argocd is running already.

Here are the steps and sample output. All these commands were run through **GITBASH**
```
$ kubectl create -f db.yaml
application.argoproj.io/petclinic-db created

$ kubectl get pvc -n app
NAME        STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS        AGE
mysql-pvc   Bound    pvc-34177950-77fb-422d-8bca-5b20b77a4477   2Gi        RWO            ebs-storage-class   49s

$ kubectl get pods -n app
NAME                        READY   STATUS    RESTARTS   AGE
mysql-db-7d8cc9857b-cfrdp   1/1     Running   0          30s
$ kubectl create -f app.yaml
application.argoproj.io/petclinic-app created
$ kubectl get pods -n app
NAME                               READY   STATUS    RESTARTS   AGE
mysql-db-7d8cc9857b-9bzdm          1/1     Running   0          17m
petclinic-deploy-b89c6d94b-66sng   1/1     Running   0          3m9s
petclinic-deploy-b89c6d94b-6zgc4   1/1     Running   0          3m9s

Once you add data, using UI check DB
$ winpty kubectl exec -it mysql-db-7d8cc9857b-9bzdm -n app ./bin/bash
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
bash-4.4# mysql -u petclinic -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 28
Server version: 8.0.34 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> use petclinic;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql>
mysql> show tables;
+---------------------+
| Tables_in_petclinic |
+---------------------+
| owners              |
| pets                |
| specialties         |
| types               |
| vet_specialties     |
| vets                |
| visits              |
+---------------------+
7 rows in set (0.00 sec)

mysql> select * from owners;
+----+----------------+----------------------+-----------------------+---------------------+------------+
| id | first_name     | last_name            | address               | city                | telephone  |
+----+----------------+----------------------+-----------------------+---------------------+------------+
|  1 | George         | Franklin             | 110 W. Liberty St.    | Madison             | 6085551023 |
|  2 | Betty          | Davis                | 638 Cardinal Ave.     | Sun Prairie         | 6085551749 |
|  3 | Eduardo        | Rodriquez            | 2693 Commerce St.     | McFarland           | 6085558763 |
|  4 | Harold         | Davis                | 563 Friendly St.      | Windsor             | 6085553198 |
|  5 | Peter          | McTavish             | 2387 S. Fair Way      | Madison             | 6085552765 |
|  6 | Jean           | Coleman              | 105 N. Lake St.       | Monona              | 6085552654 |
|  7 | Jeff           | Black                | 1450 Oak Blvd.        | Monona              | 6085555387 |
|  8 | Maria          | Escobito             | 345 Maple St.         | Madison             | 6085557683 |
|  9 | David          | Schroeder            | 2749 Blackhawk Trail  | Madison             | 6085559435 |
| 10 | Carlos         | Estaban              | 2335 Independence La. | Waunakee            | 6085555487 |
| 11 | yyyyyyyyyyyyyy | xxxxxxxxxxxxx        | xxxxxxxxxxxxxxxx      | xxxxxxxxxxxxxxx     | 999999999  | UPDATED USING GUI
| 12 | yyyyyyyyyyyyyy | zzzzzzzzzzzzzzzzzzzz | xxxxxxxxxxxxxxxxxxx   | yyyyyyyyyyyyyyyyyyy | 999999999* | UPDATED USING GUI
+----+----------------+----------------------+-----------------------+---------------------+------------+
12 rows in set (0.00 sec)
```
