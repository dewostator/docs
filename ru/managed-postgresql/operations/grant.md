---
title: "Назначение привилегий и ролей пользователям PostgreSQL"
description: "Атомарные полномочия в PostgreSQL называются привилегиями, группы полномочий — ролями. Подробнее об организации прав доступа читайте в документации PostgreSQL. Пользователь, создаваемый вместе с кластером {{ mpg-name }}, является владельцем первой базы данных в кластере. Вы можете создавать других пользователей и настраивать их права по своему усмотрению."
---

# Назначение привилегий и ролей пользователям {{ PG }}

Атомарные полномочия в **{{ PG }}** называются _привилегиями_, группы полномочий — _ролями_. Подробнее об организации прав доступа читайте в [документации {{ PG }}](https://www.postgresql.org/docs/current/user-manag.html).

Пользователь, создаваемый вместе с кластером **{{ mpg-name }}**, является владельцем первой базы данных в кластере. Вы можете [создавать других пользователей](cluster-users.md#adduser) и настраивать их права по своему усмотрению:

- [Изменить список ролей пользователя](#grant-role).
- [Выдать привилегию пользователю](#grant-privilege).
- [Отозвать привилегию у пользователя](#revoke-privilege).

## Изменить список ролей пользователя {#grant-role}

Для назначения [роли](../concepts/roles.md) пользователю используйте интерфейсы {{ yandex-cloud }}: назначение роли запросом `GRANT` отменится при следующей операции с базой.

{% list tabs %}

- Консоль управления

  1. Перейдите на страницу каталога и выберите сервис **{{ mpg-name }}**.
  1. Нажмите на имя нужного кластера и выберите вкладку **Пользователи**.
  1. В строке с именем нужного пользователя нажмите на значок ![image](../../_assets/horizontal-ellipsis.svg) и выберите пункт **Настроить**.
  1. Разверните список **Настройки СУБД** и в поле **Grants** выберите роли, которые хотите назначить пользователю.
  1. Нажмите кнопку **Сохранить**.

- CLI

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  Чтобы назначить роли пользователю кластера, передайте список нужных ролей в параметре `--grants`. Имеющиеся роли будут полностью перезаписаны: если вы хотите дополнить или уменьшить имеющийся список, сначала запросите текущие роли с информацией о пользователе командой `{{ yc-mdb-pg }} user get`.

  Чтобы назначить роли, выполните команду:

  ```
  {{ yc-mdb-pg }} user update <имя пользователя> \
         --grants=<роль1,роль2> \
         --cluster-id <идентификатор кластера>
  ```

  Имя кластера можно запросить со [списком кластеров в каталоге](cluster-list.md), имя пользователя — со [списком пользователей](cluster-users.md#list-users).

- {{ TF }}

  Чтобы назначить роли пользователю кластера:
  
    1. Откройте актуальный конфигурационный файл {{ TF }} с планом инфраструктуры.
  
        О том, как создать такой файл, см. в разделе [{#T}](cluster-create.md).

        Полный список доступных для изменения полей конфигурации пользователей кластера {{ mpg-name }} см. в [документации провайдера {{ TF }}]({{ tf-provider-link }}/mdb_postgresql_user).

    1. Найдите ресурс `yandex_mdb_postgresql_user` нужного пользователя.
    1. Добавьте атрибут `grants` со списком нужных ролей:
  
        ```hcl
        resource "yandex_mdb_postgresql_user" "<имя пользователя>" {
          ...
          name   = "<имя пользователя>"
          grants = [ "<роль1>","<роль2>" ]
          ...
        }
        ```

    1. Проверьте корректность настроек.
  
        {% include [terraform-validate](../../_includes/mdb/terraform/validate.md) %}
  
    1. Подтвердите изменение ресурсов.
  
        {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

- API

    Чтобы указать новый список необходимых ролей для пользователя, воспользуйтесь методом [update](../api-ref/User/update.md) и передайте в запросе:

    * Идентификатор кластера в параметре `clusterId`. Чтобы узнать идентификатор, [получите список кластеров в каталоге](./cluster-list.md#list-clusters).
    * Имя пользователя в параметре `userName`.
    * Список новых ролей и привилегий пользователя в параметре `grants`.

        Имеющиеся роли будут полностью перезаписаны: если вы хотите дополнить или уменьшить имеющийся список, сначала запросите текущие роли с информацией о пользователе методом [get](../api-ref/User/get.md).

    * Список полей конфигурации пользователя, которые необходимо изменить (в данном случае — `grants`), в параметре `updateMask`.

    {% include [Note API updateMask](../../_includes/note-api-updatemask.md) %}

{% endlist %}

## Выдать привилегию пользователю {#grant-privilege}

1. [Подключитесь](connect.md) к базе данных с помощью учетной записи владельца базы данных.
2. Выполните команду `GRANT`. Подробное описание синтаксиса команды смотрите в [документации {{ PG }}](https://www.postgresql.org/docs/current/sql-grant.html).


## Отозвать привилегию у пользователя {#revoke-privilege}

1. [Подключитесь](connect.md) к базе данных с помощью учетной записи владельца базы данных.
2. Выполните команду `REVOKE`. Подробное описание синтаксиса команды смотрите в [документации {{ PG }}](https://www.postgresql.org/docs/current/sql-revoke.html).

{% include [user-ro](../../_includes/mdb/mpg-user-examples.md) %}
