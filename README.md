# pavelmishutin
Перед началой установки, нужно установить Linux Oracle на VirtualBox, для этого нужно:

Иметь образ Linux Выделить 2+ ядер. Выделать 4096+ МБ оперативы. При установки операционной системы, нужно будет выбрать английский язык.

Далее переходим к установке docker с использованием grafana, вводим следующий набор команд:

1.sudo yum install wget
 устанавливает утилиту wget на вашу систему
 
![1](https://github.com/user-attachments/assets/afebe2c6-6dfc-402c-ab77-f2caf7029b02)

2.sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo

![2](https://github.com/user-attachments/assets/d6049aad-a24f-4148-84f5-7553b2b6cfab)

3.sudo yum install docker-ce docker-ce-cli containerd.io

![3](https://github.com/user-attachments/assets/a4f2f2a1-fdee-48e0-a0af-cf3ca35b067d)

4. sudo systemctl enable docker --now
   Запускаем его и разрешаем автозапуск

5. sudo yum install curl
6. COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)
Объявление переменной COMVER, полученной в результате curl запроса, хранящей в себе номер последней версии Docker Compose



7. sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
Теперь скачиваем скрипт docker-compose последней версии, используя объявленную ранее переменную и помещаем его в каталог /usr/bin

![4](https://github.com/user-attachments/assets/43b4dd85-927e-46b4-8d8e-691ae1ec199f)

8. sudo chmod +x /usr/bin/docker-compose
Предоставление прав на выполнение файла docker-compose.

9. docker-compose --version
Проверка установленной версии Docker Compose.
![5](https://github.com/user-attachments/assets/eeed4070-5abd-4d83-a99b-da340c694964)


10. git clone https://github.com/skl256/grafana_stack_for_docker.git

![6](https://github.com/user-attachments/assets/a04d66ca-c080-4d7e-8cd1-8f1a1852b8c8)

11. cd grafana_stack_for_docker

12. sudo mkdir -p /mnt/common_volume/swarm/grafana/config
 команда создаёт полный путь /mnt/common_volume/swarm/grafana/config, включая все необходимые промежуточные каталоги, если они ещё не существуют.
 
13. sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data}
 команда создаёт структуру каталогов для Grafana и связанных с ней компонентов, если они ещё не существуют.
 
14. sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}
все файлы и каталоги в указанных директориях будут переданы в собственность текущему пользователю и его группе

15. touch /mnt/common_volume/grafana/grafana-config/grafana.ini
файл grafana.ini уже существует, команда обновит его временные метки (время последнего доступа и изменения). Если файл не существует, команда создаст новый пустой файл с указанным именем по указанному пути.

16.cp config/* /mnt/common_volume/swarm/grafana/config/
команда копирует все файлы и подкаталоги из директории config в директорию /mnt/common_volume/swarm/grafana/config/

17.mv grafana.yaml docker-compose.yaml
команда переименовывает файл grafana.yaml в docker-compose.yaml. Ничего не покажет, но можно проверить при помощи команды ls

18.sudo docker compose up -d
команда создает и запускает контейнеры в фоновом режиме, используя конфигурацию из файла docker-compose.yml, с правами суперпользователя.
![7](https://github.com/user-attachments/assets/b6a4905d-8dba-4f83-939b-61ad83dff2c3)


19.sudo vi docker-compose.yaml
•команда открывает файл docker-compose.yaml в текстовом редакторе vi с правами суперпользователя, что позволяет вам редактировать его содержимое.

• Нас перекинет в текстовый редактор

• Что-бы что-то изменить в тесковом редакторе нажмите insert на клавиатуре

• Что бы сохранить что-то в этом документе нажимаем Esc пишем :wq! В этом текставом редакторе мы должны поставить node-exporter после services


20. sudo vi prometheus.yaml 
команда открывает файл prometheus.yaml в текстовом редакторе vi с правами суперпользователя.

• /mnt/common_volume/swarm/grafana/config/prometheus.yaml - исправить targets: на exporter:9100,
![8](https://github.com/user-attachments/assets/ce359f44-aab2-426e-be0e-d13056b71b3e)

<h1>Grafana</h1>
переходим на сайт localhost:3000
User & Password GRAFANA: admin
Код графаны: 3000
Код прометеуса: http://prometheus:9090
в меню выбираем вкладку Dashboards и создаем Dashboard
ждем кнопку +Add visualization, а после "Configure a new data source"
выбираем Prometheus
Connection
http://prometheus:9090
Authentication
Basic authentication
User: admin
Password: admin
Нажимаем на Save & test и должно показывать зелёную галочку
в меню выбираем вкладку Dashboards и создаем Dashboard
ждем кнопку "Import dashboard"
Find and import dashboards for common applications at grafana.com/dashboards: 1860 //ждем кнопку Load
Select Prometheus ждем кнопку "Import"

![9](https://github.com/user-attachments/assets/2465f732-d5ab-4ba3-9d14-0224a22f1a18)

<h1>VictoriaMetrics</h1>

1.cd grafana_stack_for_docker

2.sudo vi docker-compose.yaml
команда sudo открывает файл docker-compose.yaml в редакторе vi с правами суперпользователя.
В самом текстовом редакторе после prometheus вставляем
![382823376-b25ebd84-0173-4e2c-9fe5-c94b7c290a37](https://github.com/user-attachments/assets/10745382-5e2a-40a2-ae74-eae9acdfde32)

3.echo -e "# TYPE OILCOINT_metric1 gauge\nOILCOINT_metric1 0" | curl --data-binary @- http://localhost:8428/api/v1/import/prometheus  

4.curl -G 'http://localhost:8428/api/v1/query' --data-urlencode 'query=OILCOINT_metric1'
• команда делает запрос к API для получения данных по метрике OILCOINT_metric1

• команда выводит текст, который может быть использован для определения метрики в формате, совместимом с Prometheus

• команда выводит информацию о типе и значении этой метрики в формате, который может быть использован системой мониторинга Prometheus.
![11](https://github.com/user-attachments/assets/ad7fb070-6179-4331-95cd-5a3ec146f5e6)

![10](https://github.com/user-attachments/assets/12f96467-916e-41d2-9149-4a45740ffd37)

