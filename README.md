# Домашнее задание к занятию  «Защита хоста»

### Задание 1

1. Установите **eCryptfs**.
2. Добавьте пользователя cryptouser.
3. Зашифруйте домашний каталог пользователя с помощью eCryptfs.


*В качестве ответа  пришлите снимки экрана домашнего каталога пользователя с исходными и зашифрованными данными.*  

### Решение 1

Устанавливаем eCryptfs, создаем специального пользователя (добавляем ему права) и шифруем каталог пользователя:
```
sudo apt-get install ecryptfs-utils
sudo add cryptouser
sudo adduser cryptouser
sudo ecryptfs-migrate-home -u ska

```
входе видим что шифрование прошло успешно:
![1](https://github.com/SKA1010/sec_2/assets/125235217/4d138f8e-6c77-489f-9977-6a47200b1813)

При входе нас просят создать ключ восстановления данных:
![2](https://github.com/SKA1010/sec_2/assets/125235217/9c936204-f2e0-458f-a4b6-560f3d2cc53a)


### Задание 2

1. Установите поддержку **LUKS**.
2. Создайте небольшой раздел, например, 100 Мб.
3. Зашифруйте созданный раздел с помощью LUKS.

*В качестве ответа пришлите снимки экрана с поэтапным выполнением задания.*

### Решение 2

Вначале видим наш новый раздел на 1ТБ:

![3](https://github.com/SKA1010/sec_2/assets/125235217/040ba684-0296-4dad-99fc-b9224c54ff5c)

Выполняем шифрование:

![4](https://github.com/SKA1010/sec_2/assets/125235217/81ee7a38-8f20-4f3f-8433-4f8d4dcce9ab)

Монтируем раздел:

![5](https://github.com/SKA1010/sec_2/assets/125235217/5e404645-3026-4282-bdcd-556ee697803f)

Проверяем:

![6](https://github.com/SKA1010/sec_2/assets/125235217/209456ef-70f5-456e-9996-05b029d1d1ac)

Раздел зашифрован.

Листинг кода:
```
lsblk
sudo dd if=/dev/urandom of=/root/secret.key bs=1024 count=2
sudo chmod 0400 /root/secret.key
sudo apt-get install cryptsetup
cryptsetup luksFormat /dev/sdb /root/secret.key
sudo cryptsetup luksFormat /dev/sdb /root/secret.key
sudo cryptsetup luksAddKey /dev/sdb /root/secret.key --key-file=/root/secret.key
sudo cryptsetup luksOpen /dev/sdb secret --key-file=/root/secret.key
sudo cryptsetup resize secret
sudo nano /root/secret.key
sudo cryptsetup resize secret
lsblk
mkfs.ext4 /dev/mapper/secret
cryptsetup -v status secret
lsblk
cryptsetup luksDump /dev/sdb
sudo mkdir -p /secret
sudo chmod 755 /secret
mount /dev/mapper/secret /secret
df -h
lsblk
```
