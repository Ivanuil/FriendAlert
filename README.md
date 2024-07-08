# FriendAlert

### About

Friend Alert is a Telegram bot, made for School 21 students.
It allows users to subscribe for their friends entering and leaving campus.
It also provides extensive statistics on School 21 campuses and participants.

Project includes two microservices:
1. FriendAlertBot (java)
   
    FriendAlertBot gathers data from School 21 platform and sends out messages for users.
    It also logs data for analytics.

2. FriendAlertAnalytics (python)

    FriendAlertAnalytics provides analytics data visualisation

#### Project's flowchart:
```mermaid
flowchart TB
    user((User)) --> tg(Telegram) --> java(FriendAlertBot)
    
    subgraph microservices
        java --> python(FriendAlertAnalytics)
    end
    
    subgraph databases
        java --> postgres[(PostgreSQL)]
        java -- Writing log --> ch[(ClickHouse)]
        python -- Reading log --> ch
    end
```

#### Project's sequence diagram:
```mermaid
sequenceDiagram
    actor user as User
    box FriendAlert
        participant java as FriendAlertBot
        participant ch as ClickHouse
        participant python as FriendAlertAnalytics
    end
    java -->> ch: Периодически записывает пакет данных (лог)
    Note over user, java: Telegram
    user ->> java: Запрос на получение графика с аналитикой
    java ->> python: Запрос на получение графика с аналитикой
    python ->> ch: SQL запрос на необходимые данные
    python ->> java: Готовый график
    java ->> user: Готовый график
```
