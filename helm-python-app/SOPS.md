
# SOPS with helm

## Manual commands

Encrypt:

```bash
sops -e -i values.example.yaml
# or
helm secrets enc values.example.yaml
```

Decript:

```bash
sops -d -i values.example.yaml
```

Change:

```bash
helm secrets edit values.example.yaml
```

Create `das-key_age.txt`:

```bash
age-keygen -o ~/.das-key_age.txt
```

## Инструкция по созданию релиза с зашифрованными данными

0. Установить:
   1. [SOPS](https://github.com/mozilla/sops)
   2. [helm-secrets](https://github.com/jkroepke/helm-secrets)
1. Скачать `das-key_age.txt`
2. Установит переменные `SOPS_AGE_RECIPIENTS` & `SOPS_AGE_KEY_FILE`

    ```bash
    export SOPS_AGE_RECIPIENTS=age1km0n3lpecy0lm22gqpfg6z32cvs6jc3krfynlxna4h83vlt43c7su4ej3r
    export SOPS_AGE_KEY_FILE=~/.das-key_age.txt
    ```

3. Зашифровать файл `secrets.yaml`

    ```bash
    sops -e -i secrets.yaml
    # or
    helm secrets enc secrets.yaml
    ```

4. Установить helm-релиз с зашифрованными данными

    ```helm
    -f secrets://secrets.yaml
    ```
