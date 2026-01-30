# Домашнее задание к занятию «Защита сети»

### Желонкин Дмитрий

------

### Подготовка к выполнению заданий

1. Подготовка защищаемой системы:

- установите **Suricata**,
- установите **Fail2Ban**.

2. Подготовка системы злоумышленника: установите **nmap** и **thc-hydra** либо скачайте и установите **Kali linux**.

Обе системы должны находится в одной подсети.

------

### Задание 1

Проведите разведку системы и определите, какие сетевые службы запущены на защищаемой системе:

**sudo nmap -sA < ip-адрес >**
![nmap scan sa](https://github.com/deadwhitepunk/sdb-ib-03/blob/main/img/image_log_suricata_nmap_sa.png)

**sudo nmap -sT < ip-адрес >**
![nmap scan st](https://github.com/deadwhitepunk/sdb-ib-03/blob/main/img/image_log_suricata_nmap_st.png)

**sudo nmap -sS < ip-адрес >**
![nmap scan ss](https://github.com/deadwhitepunk/sdb-ib-03/blob/main/img/image_log_suricata_nmap_ss.png)

**sudo nmap -sV < ip-адрес >**
![nmap scan sv](https://github.com/deadwhitepunk/sdb-ib-03/blob/main/img/image_log_suricata_nmap_sv.png)

По желанию можете поэкспериментировать с опциями: https://nmap.org/man/ru/man-briefoptions.html.

В результате все nmap сканирования были обнаружены. В зависимости от вида сканирования surricata выдавала разный результат. В сканировании sS например, surricata алертила на конретные порты приложений. 

*В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.*

------

### Задание 2

Проведите атаку на подбор пароля для службы SSH:

**hydra -L users.txt -P pass.txt < ip-адрес > ssh**

1. Настройка **hydra**: 
 
 - создайте два файла: **users.txt** и **pass.txt**;
 - в каждой строчке первого файла должны быть имена пользователей, второго — пароли. В нашем случае это могут быть случайные строки, но ради эксперимента можете добавить имя и пароль существующего пользователя.

![user + password](https://github.com/deadwhitepunk/sdb-ib-03/blob/main/img/image_hydra_usernames+passwd.png)

Дополнительная информация по **hydra**: https://kali.tools/?p=1847.

2. Включение защиты SSH для Fail2Ban:

-  открыть файл /etc/fail2ban/jail.conf,
-  найти секцию **ssh**,
-  установить **enabled**  в **true**.
![fail2ban enable ssh](https://github.com/deadwhitepunk/sdb-ib-03/blob/main/img/image_fail2ban_enable_Ssh.png)
Дополнительная информация по **Fail2Ban**:https://putty.org.ru/articles/fail2ban-ssh.html.
Атакуем перебором с использованием наших словарий

![attack](https://github.com/deadwhitepunk/sdb-ib-03/blob/main/img/image_hydra_attack.png)


![Логи fail2ban](https://github.com/deadwhitepunk/sdb-ib-03/blob/main/img/image_fail2ban_log_ban.png)
![Логи Suricata](https://github.com/deadwhitepunk/sdb-ib-03/blob/main/img/image_hydra_surricata_log.png)

После того, как мы начали атаку наш ip попал в ban. Так как начались активные попытки перебора паролей на сервис ssh, что и видно в логах, несколько раз он дал пербрать пароль, а потом этот ip был забанен.
Surricata также определила ssh scan

*В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.*