# Домашнее задание к занятию "`Базы данных в облаке`" - `Никулин Михаил Сергеевич`



---

### Задание 1

![task_1_1.png](img%2Ftask_1_1.png)
![task_1_2.png](img%2Ftask_1_2.png)
![task_1_3.png](img%2Ftask_1_3.png)
![task_1_4.png](img%2Ftask_1_4.png)
![task_1_5.png](img%2Ftask_1_5.png)

---

## Дополнительные задания (со звездочкой*)


### Задание 2*

```
terraform {
  required_providers {
    yandex = {
      source = "yandex-cloud/yandex"
    }
  }
}


provider "yandex" {
  token = "y0_AgAAAABEoWjbAATuwQAAAADS4wxXvWtjMg8PS4KRoTr1PIANTWqz8R4"
  cloud_id = "b1gtheioau4s71j2mu0u"
  folder_id = "b1gtheioau4s71j2mu0u"
  zone = "{{ zone-id }}"
}


resource "yandex_mdb_postgresql_cluster" "bestpg" {
  name                = "bestpg"
  environment         = "PRODUCTION"
  network_id          = yandex_vpc_network.msnet.id
  security_group_ids  = [yandex_vpc_security_group.pgsql-sg.id]
  deletion_protection = true

  config {
    version = 13
    resources {
      resource_preset_id = "s2.micro"
      disk_type_id       = "network-ssd"
      disk_size          = "10"
    }
  }


  host {
    zone        = "ru-central1-a"
    subnet_id   = yandex_vpc_subnet.mssubnet-a.id
    assign_public_ip = true
  }

  host {
    zone        = "ru-central1-b"
    subnet_id   = yandex_vpc_subnet.mssubnet-b.id
    assign_public_ip = true
  }

  host {
    zone        = "ru-central1-c"
    subnet_id   = yandex_vpc_subnet.mssubnet-c.id
    assign_public_ip = true
  }
}


resource "yandex_mdb_postgresql_database" "dbms1" {
  cluster_id = yandex_mdb_postgresql_cluster.bestpg.id
  name       = "dbms1"
  owner      = "nikulinms"
}


resource "yandex_mdb_postgresql_user" "nikulinms" {
  cluster_id = yandex_mdb_postgresql_cluster.bestpg.id
  name       = "nikulinms"
  password   = "123456789"
}


resource "yandex_vpc_network" "msnet" {
  name = "msnet"
}

resource "yandex_vpc_subnet" "mssubnet-a" {
  zone           = "ru-central1-a"
  network_id     = yandex_vpc_network.msnet.id
  v4_cidr_blocks = ["10.5.0.0/16"]
}

resource "yandex_vpc_subnet" "mssubnet-b" {
  zone           = "ru-central1-b"
  network_id     = yandex_vpc_network.msnet.id
  v4_cidr_blocks = ["10.6.0.0/16"]
}

resource "yandex_vpc_subnet" "mssubnet-c" {
  zone           = "ru-central1-c"
  network_id     = yandex_vpc_network.msnet.id
  v4_cidr_blocks = ["10.7.0.0/16"]
}

resource "yandex_vpc_security_group" "pgsql-sg" {
  name      = "pgsql-sg"
  network_id     = yandex_vpc_network.msnet.id

  ingress {
    description    = "PostgreSQL"
    port           = 6432
    protocol       = "TCP"
    v4_cidr_blocks = ["0.0.0.0/0"]
  }
}

```

![task_2_1.png](img%2Ftask_2_1.png)
![task_2_2.png](img%2Ftask_2_2.png)
![task_2_3.png](img%2Ftask_2_3.png)
![task_2_4.png](img%2Ftask_2_4.png)
![task_2_5.png](img%2Ftask_2_5.png)