# mephi-linux-security-2026

Домашнее задание №1 «Изучение средств защиты ОС GNU/Linux».
Уникальный номер: **М265230**.

| Раздел | Что сделано | Команды |
|---|---|---|
| 1 | Создан пользователь `user1` (UID 1234, группа `students`), пароль нужно менять каждые 90 дней | `groupadd`, `useradd -u 1234 -G students`, `passwd`, `chage -M 90` |
| 2.1 | Найдены файлы с битом set-UID | `find / -perm -4000 -fprintf ~/monitoring_files.out '%#m %u %p\n'` |
| 2.2 | Найдены процессы с EUID = 0, запущенные обычными пользователями (`passwd`, `su`, `sudo`) | `ps -U kirill,user1 -o ruser,euser,pid,comm \| grep root` |
| 3 | Копия `cat` с битом set-UID (владелец root) читает `/etc/shadow` от имени `user1` | `sudo cp /usr/bin/cat /home/user1/cat`<br>`sudo chown root:root /home/user1/cat`<br>`sudo chmod 4755 /home/user1/cat`<br>`su - user1 -c '/home/user1/cat /etc/shadow'` |
| 4 | Копия `chown` с привилегией `CAP_CHOWN` меняет владельца файла на root от имени `user1` | `sudo cp /usr/bin/chown /home/user1/chown`<br>`sudo setcap "cap_chown=ep" /home/user1/chown`<br>`su - user1 -c '/home/user1/chown root:root ~/test.txt'` |
| 5 | `user1` может менять системное время через sudo (разрешена только команда `date`) | `visudo`: `user1 ALL=(root) /usr/bin/date` |
| 6 | Скриншот с уникальным номером, история команд | `echo`, `history` |
