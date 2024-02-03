## Домашнее задание к занятию 15 «Система сбора логов Elastic Stack»
# Задание 1
- Вам необходимо поднять в докере и связать между собой:
  - elasticsearch (hot и warm ноды);
  - logstash;
  -  kibana;
  - filebeat.
  - Logstash следует сконфигурировать для приёма по tcp json-сообщений.
  - Filebeat следует сконфигурировать для отправки логов docker вашей системы в logstash.

- В директории help находится манифест docker-compose и конфигурации filebeat/logstash для быстрого выполнения этого задания.
- Результатом выполнения задания должны быть:
  - скриншот docker ps через 5 минут после старта всех контейнеров (их должно быть 5);
  - скриншот интерфейса kibana;
  - docker-compose манифест (если вы не использовали директорию help);
  - ваши yml-конфигурации для стека (если вы не использовали директорию help).
 ## Ответ: Запуспила 
```
vagrant@vagrant:~/10-monitoring-04-elk/elk$ docker-compose up -d
Creating network "elk_elastic" with driver "bridge"
Creating network "elk_default" with the default driver
Creating volume "elk_data01" with local driver
Creating volume "elk_data02" with local driver
Creating volume "elk_data03" with local driver
Creating es-warm  ... done
Creating some_app ... done
Creating es-hot   ... done
Creating logstash ... done
Creating kibana   ... done
Creating filebeat ... done
vagrant@vagrant:~/10-monitoring-04-elk/elk$ docker-compose ps
  Name                Command               State                                  Ports
------------------------------------------------------------------------------------------------------------------------es-hot     /bin/tini -- /usr/local/bi ...   Up      0.0.0.0:9200->9200/tcp,:::9200->9200/tcp,
                                                    0.0.0.0:9300->9300/tcp,:::9300->9300/tcp
es-warm    /bin/tini -- /usr/local/bi ...   Up      9200/tcp, 9300/tcp
filebeat   /usr/bin/tini -- /usr/loca ...   Up
kibana     /bin/tini -- sh -c sleep 3 ...   Up      0.0.0.0:5601->5601/tcp,:::5601->5601/tcp
logstash   /usr/local/bin/docker-entr ...   Up      0.0.0.0:5044->5044/tcp,:::5044->5044/tcp,
                                                    0.0.0.0:5046->5046/tcp,:::5046->5046/tcp, 9600/tcp
some_app   python3 /opt/run.py              Up
vagrant@vagrant:~/10-monitoring-04-elk/elk$ docker ps
CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS          PORTS                                                                                            NAMES
d0f2f18711e9   elastic/filebeat:8.7.0   "/usr/bin/tini -- /u…"   22 seconds ago   Up 19 seconds                                                                                                    filebeat
71be177c30d8   kibana:8.7.0             "/bin/tini -- sh -c …"   26 seconds ago   Up 22 seconds   0.0.0.0:5601->5601/tcp, :::5601->5601/tcp                                                        kibana
76c9023596df   logstash:8.7.0           "/usr/local/bin/dock…"   26 seconds ago   Up 21 seconds   0.0.0.0:5044->5044/tcp, :::5044->5044/tcp, 0.0.0.0:5046->5046/tcp, :::5046->5046/tcp, 9600/tcp   logstash
3614022f07f4   elasticsearch:8.7.0      "/bin/tini -- /usr/l…"   29 seconds ago   Up 25 seconds   0.0.0.0:9200->9200/tcp, :::9200->9200/tcp, 0.0.0.0:9300->9300/tcp, :::9300->9300/tcp             es-hot
48e487895f04   python:3.9-alpine        "python3 /opt/run.py"    31 seconds ago   Up 28 seconds                                                                                                    some_app
905a3af0e4be   elasticsearch:8.7.0      "/bin/tini -- /usr/l…"   31 seconds ago   Up 28 seconds   9200/tcp, 9300/tcp                                                                               es-warm
vagrant@vagrant:~/10-monitoring-04-elk/elk$ docker exec -it es-warm /bin/bash
elasticsearch@905a3af0e4be:~$ cd /usr/share/elasticsearch
elasticsearch@905a3af0e4be:~$ bin/elasticsearch-create-enrollment-token --scope kibana
WARNING: Owner of file [/usr/share/elasticsearch/config/users] used to be [root], but now is [elasticsearch]
WARNING: Owner of file [/usr/share/elasticsearch/config/users_roles] used to be [root], but now is [elasticsearch]

ERROR: Failed to determine the health of the cluster.
elasticsearch@905a3af0e4be:~$
vagrant@vagrant:~/10-monitoring-04-elk/elk$ curl -X GET "localhost:9200/_cluster/health?pretty"

{
  "cluster_name" : "es-docker-cluster",
  "status" : "red",
  "timed_out" : false,
  "number_of_nodes" : 2,
  "number_of_data_nodes" : 2,
  "active_primary_shards" : 0,
  "active_shards" : 0,
  "relocating_shards" : 0,
  "initializing_shards" : 0,
  "unassigned_shards" : 0,
  "delayed_unassigned_shards" : 0,
  "number_of_pending_tasks" : 2,
  "number_of_in_flight_fetch" : 0,
  "task_max_waiting_in_queue_millis" : 422,
  "active_shards_percent_as_number" : "NaN"
}

elasticsearch@4bdf507bc6bf:~$ bin/elasticsearch-create-enrollment-token --scope kibana
ERROR: [xpack.security.enrollment.enabled] must be set to `true` to create an enrollment token
```
  ###Подскажите  как добыть токен. Что не так делаю? 
  [Логи]https://github.com/EVolgina/elk/blob/main/%D0%BB%D0%BE%D0%B3%D0%B8.docx
  ![param](https://github.com/EVolgina/elk/blob/main/param.PNG)
  ![1](https://github.com/EVolgina/elk/blob/main/5601.PNG)
# Задание 2
- Перейдите в меню создания index-patterns в kibana и создайте несколько index-patterns из имеющихся.
- Перейдите в меню просмотра логов в kibana (Discover) и самостоятельно изучите, как отображаются логи и как производить поиск по логам.
- В манифесте директории help также приведенно dummy-приложение, которое генерирует рандомные события в stdout-контейнера. Эти логи должны порождать индекс logstash-* в elasticsearch. Если этого индекса нет — воспользуйтесь советами и источниками из раздела «Дополнительные ссылки» этого задания.
                                                                           
