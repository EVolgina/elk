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
 '''
vagrant@vagrant:~/10-monitoring-04-elk/elk$ docker-compose up -d
some_app is up-to-date
es-warm is up-to-date
es-hot is up-to-date
logstash is up-to-date
kibana is up-to-date
filebeat is up-to-date
vagrant@vagrant:~/10-monitoring-04-elk/elk$ docker ps
CONTAINER ID   IMAGE                    COMMAND                  CREATED      STATUS          PORTS                                                                                            NAMES
453e4fe57430   elastic/filebeat:8.7.0   "/usr/bin/tini -- /u…"   4 days ago   Up 4 days                                                                                                        filebeat
47d77834779b   logstash:8.7.0           "/usr/local/bin/dock…"   4 days ago   Up 4 days       0.0.0.0:5044->5044/tcp, :::5044->5044/tcp, 0.0.0.0:5046->5046/tcp, :::5046->5046/tcp, 9600/tcp   logstash
6388ef460d20   kibana:8.7.0             "/bin/tini -- sh -c …"   4 days ago   Up 4 days       0.0.0.0:5601->5601/tcp, :::5601->5601/tcp                                                        kibana
50443f2aa616   elasticsearch:8.7.0      "/bin/tini -- /usr/l…"   4 days ago   Up 18 minutes   0.0.0.0:9200->9200/tcp, :::9200->9200/tcp, 0.0.0.0:9300->9300/tcp, :::9300->9300/tcp             es-hot
f3957415fb54   elasticsearch:8.7.0      "/bin/tini -- /usr/l…"   4 days ago   Up 4 days       9200/tcp, 9300/tcp    es-warm
37d1d7a3f108   python:3.9-alpine        "python3 /opt/run.py"    4 days ago   Up 4 days                                                                                                        some_app
'''
    
# Задание 2
- Перейдите в меню создания index-patterns в kibana и создайте несколько index-patterns из имеющихся.
- Перейдите в меню просмотра логов в kibana (Discover) и самостоятельно изучите, как отображаются логи и как производить поиск по логам.
- В манифесте директории help также приведенно dummy-приложение, которое генерирует рандомные события в stdout-контейнера. Эти логи должны порождать индекс logstash-* в elasticsearch. Если этого индекса нет — воспользуйтесь советами и источниками из раздела «Дополнительные ссылки» этого задания.
                                                                           
