**Docker -** это платформа для создания, развертывания и запуска приложений в изолированных контейнерах. Контейнеры позволяют упаковать приложение и его зависимости вместе, обеспечивая независимую среду выполнения на разных компьютерах или серверах.

Для создания образа мы обращемся к демон-процессу и образ уже собирается через демона

Берется докер файл, в нем прописываются инструкции, докер их собирает в докер образ. Каждая инструкция создает промежуточный слой образа. Инструкции кэшируются в образах.

Преимущество контейнеризации в том что у нас допустим есть конкретно микросервис, одно приложение, и в контейнере лежит только то что нужно приложению чтобы запуститься, и как раз докер служит удобным средством создания таких контейнеров.

**Без инструкции** **FROM** **не может существовать докер-файл**. FROM указывает базовый образ, на основе которого мы строим свой образ.

Инструкция **COPY** копирует файлы из системы в образ

Docker run – создание контейнера и запуск

RUN инструкция задает команды, которые выполняются при сборке контейнера.

CMD – задает команду которая выполнится при старте контейнера.

## *CI/CD*

 CI(Continueons Integration) - когда разработчики регулярно пушат изменения в репозиторий, после чего автоматический инструмент, собирает, тестирует, оповещает если что то пошло не так 

  CD(Continueons Delivery) - как продолжение CI, где все это начинает автоматически разворачиваться там где нужно.

Все это происходит в пайплайнах. То есть есть джобы - как одна какая-то задача на выполнение в стейдже (Пайплайны также могут быть для разных веток)
Основные этапа пайплайна 
1. получение кода -> сборка, создание образов, контейнеров и т.д -> происходит развертывание приложения на продакшне -> наблюдение за развернутой версией 

У нас артефакты деплоили лид, то есть гитлабовский ci cd билдил, тестировал и деплоил в кубер

## ***KUBERNETES***

**- система управления контейнерами.**

- Служит для автоматического развертывания приложений на разных серверах
    
- Для автоматического масштабирования развернутых приложений (То есть если какое то приложение возрастает, возможно создание доп контейнеров, затем их удаление)
    
- Для мониторинга и проверки работоспособности контейнеров (если работа в каком то контейнере остановилась, то выполнится пересоздание контейнера)
    

В Kubernetes существуют **поды** – то есть такой самый маленький элемент, и как раз они создаются для хранения контейнеров и их запуска. Под состоит из одного или нескольких контейнеров, для каждого пода выделяется место на диске, то есть общее место хранения данных контейнеров одного пода.

Кластер Kubernetes:

В кластере находятся узлы, где нагрузка распределяется по этим узлам (серверам) и вот в каждом узле в рамках пода запускаются контейнеры.

В кластере выбирается один главный узел (MasterNode) и этот узел контролирует WorkerNodes.

В узлах присутствуют такие сервисы как:

**Kubelet** – сервис отвечающий за комуникацию между узлами кластера

**Kubeproxy** – отвечает за сетевые ресурсы в рамках каждого узла

**Компоненты** **MasterNode**

**API** **Server** – через него происходит общение между мастер нодой и воркер через kubelet

**Scheduler** – распределять нагрузку между узлами

**Controller** **Manager** – отвечает за работу подов, отвечает за работу таких контроллеров как **Deployment** (отвечает за обновление подов) **ReplicaSet** – за копии(для гарантии доступности) **Jobs** – отвечают за выполнение задачи.

**ETCD** – хранит инфу о настройках и состоянии кластера

Для управления кластером используется kubectl через командную строку