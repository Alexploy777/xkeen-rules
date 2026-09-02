# XKeen / Mihomo — Custom Unblock Rules

Собственный набор правил для выборочной маршрутизации заблокированных или требующих VPN ресурсов через **XKeen + Mihomo**.

Основной список правил:

```text
unblock.list
```

## Назначение

Постоянные пользовательские правила хранятся в GitHub и загружаются Mihomo как `rule-provider`.

В конфигурации используется:

```yaml
user-remote@classical:
  <<: *classical
  url: https://raw.githubusercontent.com/Alexploy777/xkeen-rules/refs/heads/main/unblock.list
```

Для быстрых и временных правил дополнительно используется локальный блок:

```yaml
user@classical:
  <<: *inline
  payload:
    - DOMAIN-SUFFIX,example.com
```

Оба пользовательских набора направляются через группу:

```text
Заблок. сервисы
```

---

## Добавление домена и всех его поддоменов

```text
DOMAIN-SUFFIX,example.com
```

Правило включает:

```text
example.com
www.example.com
api.example.com
login.example.com
```

Это основной рекомендуемый вариант для добавления целого сервиса.

---

## Добавление только конкретного домена

```text
DOMAIN,api.example.com
```

Совпадёт только с:

```text
api.example.com
```

---

## Один IPv4

Для одного IP используется `/32`:

```text
IP-CIDR,203.0.113.25/32
```

---

## Диапазон IPv4

```text
IP-CIDR,203.0.113.0/24
```

Например:

```text
IP-CIDR,10.20.0.0/16
IP-CIDR,203.0.113.128/25
```

---

## Один IPv6

Для одного IPv6 используется `/128`:

```text
IP-CIDR6,2001:db8::1/128
```

---

## Диапазон IPv6

```text
IP-CIDR6,2001:db8::/32
```

---

## Краткая шпаргалка

| Что требуется | Правило |
|---|---|
| Домен и все поддомены | `DOMAIN-SUFFIX,example.com` |
| Только конкретный домен | `DOMAIN,api.example.com` |
| Один IPv4 | `IP-CIDR,203.0.113.25/32` |
| Диапазон IPv4 | `IP-CIDR,203.0.113.0/24` |
| Один IPv6 | `IP-CIDR6,2001:db8::1/128` |
| Диапазон IPv6 | `IP-CIDR6,2001:db8::/32` |

---

## Куда добавлять правила

### Постоянные правила

Добавлять в:

```text
unblock.list
```

Это основной способ.

### Временные или тестовые правила

Добавлять непосредственно в конфигурацию:

```yaml
user@classical:
  <<: *inline
  payload:
    - DOMAIN-SUFFIX,example.com
    - DOMAIN,api.example.net
    - IP-CIDR,8.8.8.8/32
```

После проверки рабочее правило желательно перенести в `unblock.list`.

---

## Приоритет правил

Mihomo проверяет правила сверху вниз и использует **первое совпадение**.

Поэтому пользовательские правила размещены выше встроенных категорий:

```text
user@classical
user-remote@classical
        ↓
AI / YouTube / Discord / другие категории
        ↓
refilter@domain
        ↓
MATCH,DIRECT
```

Таким образом правила из этого репозитория могут переопределять встроенные категории Mihomo.

---

## Обновление правил после изменения GitHub

Mihomo периодически обновляет `rule-provider` автоматически.

Чтобы загрузить изменения сразу:

```text
Zashboard
→ Провайдеры правил
→ user-remote@classical
→ ↻ Обновить
```

После обновления можно нажать значок просмотра и убедиться, что новые правила загрузились.

---

## Проверка маршрутизации

В Zashboard открыть:

```text
Соединения
```

Найти нужный домен.

Для правил из `unblock.list` в цепочке должно быть:

```text
Заблок. сервисы → выбранный VPN/Proxy
```

---

## Пример `unblock.list`

```text
DOMAIN-SUFFIX,openai.com
DOMAIN-SUFFIX,chatgpt.com
DOMAIN,js.stripe.com
IP-CIDR,203.0.113.25/32
IP-CIDR,203.0.113.0/24
```
