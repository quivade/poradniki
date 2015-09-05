# Praca zdalna na serwerach Quivade
Połączenie zdalne są możliwe z pomocą
ssh [Secure Shell](https://en.wikipedia.org/wiki/Secure_Shell) oraz
VNC ([Virtual Network Computing](https://en.wikipedia.org/wiki/Virtual_Network_Computing)).

## SSH
Połączenie ssh pozwala przede wszystkim na prace w trybie tekstowym.
Możliwe jest także uruchamianie programów graficznych poprzez uruchomienie sesji z przekazywaniem X,
ale ta opcja zostanie najprawdopodobniej wyłączona w niedalekiej przyszłości.
Serwer ssh nasłuchuje na porcie 39934 i *nie wymaga* połączenia poprzez VPN
(co także zostanie zmienione w przyszłości).

Popularnym klientem ssh dla systemu Windows jest [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/).
Kazda dystrubucja linuksa zawiera juz program `ssh` z pakietu openssh.

## VNC
Możliwe są dwa rodzaje połączenia VNC - systemowe oraz indywidualne użytkownika.
Oba przypadki wymagają najpierw nawiązania bezpiecznego połączenia VPN
([Virtual Private Network](https://en.wikipedia.org/wiki/Virtual_private_network)).
Jako adres serwera VNC należy podać prywatny adres serwera wewnątrz VPNa `10.8.0.1`.
Wybór połączenia dokonuję się poprzez połączenia na odpowiednim porcie.

### Systemowe połączenie VNC
Serwer systemowy czeka na połączenia na porcie typowym dla VNC: `5900`.
Po połączeniu się z serwerem powiat nas ekran logowania i poprosi o wybranie użytkownika,
lub podanie loginu jeżeli nie znajdziemy naszego na liście, oraz hasła.
Na panelu w prawym górnym rogu ekranu możemy wybrać język oraz rodzaj sesji.
Obecnie zainstalowane są:
* Gnome
* i3
* awesome (prawdopodobnie zostanie usunięte)

Sesja systemowa jest aktywna tylko do rozłączenia/wylogowania.
Jeżeli potrzebna jest sesja która będzie aktywna dłużej i będzie można do niej powrócić
należy skorzystać z serwera VNC uruchamianego przez użytkownika.

### Serwer VNC użytkownika
W celu łatwiejszego korzystania i zarządzania serwerem VNC przygotowano
zestaw plików dla [systemd](https://en.wikipedia.org/wiki/Systemd).
Do kontroli usług uruchamianaych za pomocą systemd używa się polcenia `systemctl`.
Użytkownicy nie uprzywilejowani muszą podać jeszcze parametr `--user`.
Podstawowe polecenia wydawane `systemctl` to:
* `status` pokazuje stan usługi podanej jako parametr lub wszystkich usług jeżeli nie podano parametru
* `start` uruchamia usługę podaną jako parametr
* `status` zatrzymuję usługę podaną jako parametr

Przykład (wyświetla usługi uruchomione użytkownika):
```shell
systemctl --user status
```

Sesję użytkownika uruchamia się poleceniem:
```shell
systemctl --user start session@typ.service
```

Ponieważ sesja użytkownika jest indywidualna dla niego,
po podłączeniu nie pokaże się ekran logowania tylko pulpit.
Dlatego należy zastąpić *typ* w podanym wyżej poleceniu rodzajem sesji jaką chcemy uruchomić.
Aktualnie dostępne warianty to:
* `i3` dla menedżera okien [i3](https://i3wm.org)
* `gnome-session` dla [Gnome](https://www.gnome.org)

Usługa session@.service uruchomi usługę od której zależy xvnc.service.
Uruchomiony serwer VNC będzie nasłuchiwał na porcie `5900 + UID`.
