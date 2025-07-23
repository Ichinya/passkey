# 🔐 passkey — шифрование паролей локально

CLI-утилита для безопасного хранения и расшифровки паролей. Утилита для безопасного шифрования и расшифровки паролей в консоли. Не требует внешнего сервера. Ключ хранится только у вас.

Работает через `openssl`, не требует внешнего сервера, использует переменную окружения `PASSCRYPT_KEY`.

## Утилита `passcrypt` на Go

Использование утилиты `passcrypt` для шифрования и дешифрования паролей.

```bash
# Шифрование (e = encrypt)
PASSCRYPT_KEY="ключ" ./passkey e "my-password"

# Расшифровка (d = decrypt)
PASSCRYPT_KEY="ключ" ./passkey d "base64-cipher"

# Без переменных
go run main.go encrypt
```

### Сборка

```bash
go build -o passkey go/main.go
```

Сделать исполняемый файл можно с помощью команды `go build`, которая создаст файл `passkey` в текущей директории.

```shell
chmod +x passkey.sh
sudo mv passkey.sh /usr/local/bin/passkey

```

## Использование через скрипт

```bash
export PASSCRYPT_KEY="superkey"

# Шифрование
./passkey.sh e "my-password"
# => U2FsdGVkX1+...

# Расшифровка
./passkey.sh d "U2FsdGVkX1+..."
# => my-password
```

### Запуск с curl/wget

```shell
PASSCRYPT_KEY="yourkey" bash <(wget -qO- https://raw.githubusercontent.com/ichinya/passkey/main/shell/passkey.sh) d "ciphertext"
```

### Установка в систему

```bash
curl -sSL https://raw.githubusercontent.com/ichinya/passkey/main/shell/install.sh | bash
```

✅ Примеры использования

```bash
export PASSCRYPT_KEY="mykey"
# Шифрование пароля
passkey e "my-secret-password"
# Расшифровка пароля
passkey d "U2FsdGVkX1+..."
```

Если не хочешь устанавливать — можно запускать так:

```bash
PASSCRYPT_KEY="mykey" bash <(curl -s https://raw.githubusercontent.com/ichinya/passkey/main/shell/passkey.sh) e "password"
```

## 🎛️ Уровни сложности генерации

| Уровень    | Пример символов           | Назначение                      |
| ---------- | ------------------------- | ------------------------------- |
| `low`      | `abcABC123`               | Простой, удобный, читаемый      |
| `medium`   | `abcABC123@#-_+=`         | Подходит для большинства сайтов |
| `strong`   | `abcABC123!@#$%^&*()[]{}` | Для менеджеров паролей, API     |
| `paranoid` | `abc123🔐💡你好†±λ@#🧠€`    | Максимум энтропии, непроизносим |

---

## 🔧 Использование:

```bash
passkey g --length=24 --level=strong
passkey g --level=low
passkey g -l 64 -L paranoid
```

### Алиасы:

* `-l` → длина
* `-L` → уровень сложности (`low`, `medium`, `strong`, `paranoid`)
* Уровень по умолчанию: `medium`
* Длина по умолчанию: `16`

---

## 🧪 Тесты:

* Генерация 1000 паролей на каждый уровень
* Проверка длины
* Проверка уникальности
* Проверка состава символов по допустимому набору
