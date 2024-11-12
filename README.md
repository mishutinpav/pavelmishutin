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
![4](https://github.com/user-attachments/assets/788698da-5555-40cb-a09a-2c21cf647f73)

7. sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
Теперь скачиваем скрипт docker-compose последней версии, используя объявленную ранее переменную и помещаем его в каталог /usr/bin
![5](https://github.com/user-attachments/assets/a290ae7c-8385-4ba2-970d-290fe8214f87)

8. sudo chmod +x /usr/bin/docker-compose
Предоставление прав на выполнение файла docker-compose.

9. docker-compose --version
Проверка установленной версии Docker Compose.
![6](https://github.com/user-attachments/assets/e46b7a1e-e85b-4b4d-aa55-bc5dc7c74dcc)

10. git clone https://github.com/skl256/grafana_stack_for_docker.git
![7](https://github.com/user-attachments/assets/6a7c5f5d-dbfd-4a49-8db4-9be50450265d)

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
![8](https://github.com/user-attachments/assets/1398096e-4651-4359-9955-82820cd27081)

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
