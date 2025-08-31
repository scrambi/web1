# Test helm

## Install / Upgrade

`helm upgrade --install python . -f values-test.yaml`

## Delete

`helm uninstall python`

## Settings

### Config map

Все что в папке `configs` попадет в под -> папка `/configs`

### env & secret

Все описываетя в `values`. Пример:

```yaml
env:
  # non sensitive variables
  public:
    DATABASE_URL: localhost
    DATABASE_USER: user
  # sensitive variables
  secret:
    DATABASE_PASSWORD: pa$$w0rd
    SUPER_SECRET: super_secret_text
```

### Affinity

Зашит в `deployment`. Использует `fullname` для ограничения - один под на одном хосте.

TODO ниже:

- нормальный affinity (рабочий)
- выпуск серта через аннотацию
