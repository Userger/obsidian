https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/

## Сетевой интерфейс ввода-вывода низкоуровнего кода asyncio

[Объекты `asyncio.Transport` и `asyncio.Protocol`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/ "Объекты Transport и Protocol в цикле событий asyncio в Python.") модуля `asyncio` используются низкоуровневыми API-интерфейсами [цикла событий](https://docs-python.ru/standart-library/modul-asyncio-python/tsikly-sobytij-modulja-asyncio/ "Создание и получение текущего цикла событий, модуль asyncio в Python."), такими как [`loop.create_connection()`](https://docs-python.ru/standart-library/modul-asyncio-python/sozdanie-tcp-udp-unix-soedinenij-modulem-asyncio/ "Создание TCP, UDP и Unix соединений из цикла событий asyncio в Python.").

Транспорт и протоколы используют стиль программирования на основе обратного вызова и обеспечивают высокопроизводительную реализацию сетевых протоколов или протоколов IPC (например HTTP).

По сути, транспорты и протоколы [модуля `asyncio`](https://docs-python.ru/standart-library/modul-asyncio-python/ "Модуль asyncio в Python, асинхронное выполнение кода.") следует использовать только в библиотеках и фреймворках, а не в высокоуровневых асинхронных приложениях.

На самом высоком уровне транспорт (`asyncio.Transport`) связан с тем, как передаются байты, в то время как протокол (`asyncio.Protocol`) определяет, какие байты передать и в некоторой степени, когда нужно передавать.

То же самое можно сказать и по-другому: транспорт - это абстракция для сокета (или аналогичной конечной точки ввода-вывода), а протокол - это абстракция для приложения с точки зрения транспорта.

Еще одна точка зрения состоит в том, что вместе интерфейсы транспорта и протокола определяют абстрактный интерфейс для использования сетевого ввода-вывода и межпроцессного ввода-вывода.

Между объектами транспорта (transport) и объектами протокола (protocol) всегда существует соотношение 1:1. Протокол вызывает методы транспорта для отправки данных, а транспорт в свою очередь, вызывает методы протокола для передачи полученных данных.

Большинство [методов цикла событий](https://docs-python.ru/standart-library/modul-asyncio-python/tsikly-sobytij-modulja-asyncio/ "Создание и получение текущего цикла событий, модуль asyncio в Python."), ориентированных на соединения (таких как [`loop.create_connection()`](https://docs-python.ru/standart-library/modul-asyncio-python/sozdanie-tcp-udp-unix-soedinenij-modulem-asyncio/ "Создание TCP, UDP и Unix соединений из цикла событий asyncio в Python.")), обычно принимают аргумент `protocol_factory`, используемый для создания объекта протокола для принятого соединения, представленного объектом транспорта. Такие методы обычно возвращают [кортеж](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-tuple-kortezh/ "Кортеж tuple в Python.") `(transport, protocol)`.

### Содержание:

- Транспорт:
    - `asyncio.BaseTransport` [базовый транспорт](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#asyncio.BaseTransport),
    - `asyncio.WriteTransport()` [транспорт для подключений, только для записи](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#asyncio.WriteTransport),
    - `asyncio.ReadTransport()` [транспорт для подключений, только для чтения](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#asyncio.ReadTransport),
    - `asyncio.Transport()` [двунаправленный транспорт, например TCP-соединение](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#asyncio.Transport),
    - `asyncio.DatagramTransport()` [транспорт для UDP соединений](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#asyncio.DatagramTransport),
    - `asyncio.SubprocessTransport()` [транспорт для связи между родительским и дочерним процессом](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#asyncio.SubprocessTransport),
- Протоколы:
    - `asyncio.BaseProtocol` [базовый протокол](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#asyncio.BaseProtocol),
    - `asyncio.Protocol()` [для реализации TCP, Unix sockets и т. д. протоколов](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#asyncio.Protocol),
    - `asyncio.BufferedProtocol()` [для реализации протоколов с ручным управлением буфера](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#asyncio.BufferedProtocol),
    - `asyncio.DatagramProtocol()` [для реализации UDP протоколов](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#asyncio.DatagramProtocol),
    - `asyncio.SubprocessProtocol()` [для реализации однонаправленных `pipe` каналов](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#asyncio.SubprocessProtocol),
- Примеры использования `Transport` и `Protocol` модуля `asyncio`:
    - [Пример UDP эхо-сервера](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#UDP-server),
    - [Пример UDP эхо-клиента](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#UDP-client),
    - [Пример подключения к существующему сокету](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#socket).

---

## Транспорт.

Транспорт - это классы, предоставляемые модулем `asyncio` для абстрагирования различных типов каналов связи.

Объекты транспорта всегда создаются с помощью цикла событий `asyncio`.

Модуль `asyncio` реализует транспорт для TCP, UDP, SSL и каналов подпроцесса. Доступные для транспорта методы зависят от его типа.

Классы транспортов [не являются потокобезопасными](https://docs-python.ru/standart-library/modul-asyncio-python/parallelizm-mnogopotochnost-module-asyncio/ "Параллелизм и многопоточность в цикле событий asyncio в Python.").

### _`asyncio.BaseTransport`_:

Класс `asyncio.BaseTransport` представляет собой базовый класс для всех транспортов. Содержит методы, общие для всех транспортов `asyncio`.

- `BaseTransport.close()` [закрывает транспорт](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseTransport.close),
- `BaseTransport.is_closing()` [проверяет, закрывает ли транспорт](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseTransport.is_closing),
- `BaseTransport.get_extra_info()` [возвращает информации о транспорте](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseTransport.get_extra_info),
- `BaseTransport.set_protocol()` [устанавливает новый протокол](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseTransport.set_protocol),
- `BaseTransport.get_protocol()` [возвращает текущий протокол](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseTransport.get_protocol).

##### _`BaseTransport.close()`_:

Метод `BaseTransport.close()` закрывает транспорт.

Если транспорт имеет буфер для исходящих данных, то буферные данные будут сброшены асинхронно. Больше никаких данных не будет получено. После того, как все буферные данные будут сброшены, метод [`Protocol.connection_lost()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseProtocol.connection_lost) будет вызываться с `None` в качестве аргумента.

##### _`BaseTransport.is_closing()`_:

Метод `BaseTransport.is_closing()` возвращает `True`, если транспорт закрывается или уже закрыт.

##### _`BaseTransport.get_extra_info(name, default=None)`_:

Метод `BaseTransport.get_extra_info()` возвращает информации о транспорте или базовых ресурсах, которые он использует.

Аргумент `name` - это строка, представляющая часть информации о транспорте.

по умолчанию значение для возврата, если информация недоступна, или если транспорт не поддерживает запрос с данной третьей стороной реализации цикла событий или на текущей платформе.

Например, следующий код пытается получить базовый объект транспорта сокета:

sock = transport.get_extra_info('socket')
if sock is not None:
    print(sock.getsockopt(...))

Категории информации, которую можно запросить для некоторых транспортов:

- `socket`:
    - `'peername'`: удаленный адрес, к которому подключен сокет, результат [`socket.socket.getpeername()`](https://docs-python.ru/standart-library/modul-socket-setevoj-interfejs-python/obekt-socket-modulja-socket/ "Объект Socket модуля socket в Python.") или `None`;
    - `'socket'`: экземпляр [socket.socket()](https://docs-python.ru/standart-library/modul-socket-setevoj-interfejs-python/funktsija-socket-modulja-socket-sozdaet-novyj-soket/ "Функция socket() модуля socket, создает новый сокет в Python.");
    - `'sockname'`: собственный адрес сокета, результат `socket.socket.getsockname()`][socket.obj](https://docs-python.ru/standart-library/modul-socket-setevoj-interfejs-python/obekt-socket-modulja-socket/ "Объект Socket модуля socket в Python.").
- `SSL socket`:
    - `'compression'`: алгоритм сжатия используется как строка или `None`, если соединение не сжато; результат `ssl.SSLSocket.compression()`;
    - `'cipher'`: [кортеж](https://docs-python.ru/tutorial/osnovnye-vstroennye-tipy-python/tip-dannyh-tuple-kortezh/ "Кортеж tuple в Python.") из трех значений, содержащий имя используемого шифра, версию протокола SSL, определяющую его использование, и количество используемых секретных битов; результат `ssl.SSLSocket.cipher()`;
    - `'peercert'`: одноранговый сертификат; результат `ssl.SSLSocket.getpeercert()`;
    - `'sslcontext'`: экземпляр `ssl.SSLContext`;
    - `'ssl_object'`: `ssl.SSLObject` или `ssl.SSLSocket` экземпляр.
- `pipe`:
    - `'pipe'`: объект канала pipe.
- `subprocess`:
    - `'subprocess'`: экземпляр [`subprocess.Popen`](https://docs-python.ru/standart-library/modul-subprocess-python/obekt-popen-modulja-subprocess/ "Объект Popen модуля subprocess в Python.")

##### _`BaseTransport.set_protocol(protocol)`_:

Метод `BaseTransport.set_protocol()` устанавливает новый протокол `protocol`.

Переключение протокола должно выполняться только тогда, когда оба протокола задокументированы и поддерживают этот транспорт.

##### _`BaseTransport.get_protocol()`_:

Метод `BaseTransport.get_protocol()` возвращает текущий протокол.

### _`asyncio.WriteTransport(BaseTransport)`_:

Класс `asyncio.WriteTransport()` представляет собой базовый транспорт для подключений, только для записи.

Экземпляры класса `WriteTransport()` возвращаются из метода цикла событий [`loop.connect_write_pipe()`](https://docs-python.ru/standart-library/modul-asyncio-python/zapusk-podprotsessov-tsikla-sobytij-asyncio/ "Создание субпроцесса из цикла событий asyncio в Python."), а также используются методами, связанными с подпроцессами, такими как [`loop.subprocess_exec()`](https://docs-python.ru/standart-library/modul-asyncio-python/zapusk-podprotsessov-tsikla-sobytij-asyncio/ "Создание субпроцесса из цикла событий asyncio в Python.").

- `WriteTransport.abort()` [немедленно закрывает транспорт](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#WriteTransport.abort),
- `WriteTransport.can_write_eof()` [проверяет поддержку полузакрытых соединений](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#WriteTransport.can_write_eof),
- `WriteTransport.get_write_buffer_size()` [текущий размер выходного буфера](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#WriteTransport.get_write_buffer_size),
- `WriteTransport.get_write_buffer_limits()` [получает лимиты буфера записи](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#WriteTransport.get_write_buffer_limits),
- `WriteTransport.set_write_buffer_limits()` [устанавливает лимиты буфера записи](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#WriteTransport.set_write_buffer_limits),
- `WriteTransport.write()` [записывает несколько байтов](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#WriteTransport.write),
- `WriteTransport.writelines()` [записывает список](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#WriteTransport.writelines),
- `WriteTransport.write_eof()` [закрывает записывающую сторону транспорта](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#WriteTransport.write_eof).

##### _`WriteTransport.abort()`_:

Метод `WriteTransport.abort()` немедленно закрывает транспорт, не дожидаясь завершения незавершенных операций. Буферизованные данные будут потеряны. Больше никаких данных не будет.

В конечном итоге метод протокола [`Protocol.connection_lost()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseProtocol.connection_lost) будет вызываться с параметром `None` в качестве аргумента.

##### _`WriteTransport.can_write_eof()`_:

Метод `WriteTransport.can_write_eof()` возвращает `True`, если транспорт поддерживает [`Transport.write_eof()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#WriteTransport.write_eof) и `False`, если нет.

##### _`WriteTransport.get_write_buffer_size()`_:

Метод `WriteTransport.get_write_buffer_size()` возвращает текущий размер выходного буфера, используемого транспортом.

##### _`WriteTransport.get_write_buffer_limits()`_:

Метод `WriteTransport.get_write_buffer_limits()` получает маркеры пределов `low` и `high` для управления потоком записи. Возвращает кортеж `(low, high)`, где `low` и `high` - положительное количество байтов.

Чтобы установить эти предельные значения используйте метод [`Transport.set_write_buffer_limits()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#WriteTransport.set_write_buffer_limits).

##### _`WriteTransport.set_write_buffer_limits(high=None, low=None)`_:

Метод `WriteTransport.set_write_buffer_limits()` устанавливает верхний `high` и нижний `low` маркеры пределов для управления потоком записи.

Эти два значения измеряются в байтах и определяют, когда вызываются методы протокола [`protocol.pause_writing()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseProtocol.pause_writing) и [`protocol.resume_writing()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseProtocol.resume_writing). Если указан, нижний `low` предел, то он должен быть меньше или равен верхнему пределу `high`. Оба предела не могут быть отрицательными.

Метод протокола [`protocol.pause_writing()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseProtocol.pause_writing) вызывается, когда размер буфера становится больше или равен максимальному значению `high`. Если запись была приостановлена, то вызывается функция [`protocol.resume_writing()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseProtocol.resume_writing), когда размер буфера становится меньше или равен нижнему `low` значению.

Значения по умолчанию зависят от реализации. Если указан только `high`, то `low` по умолчанию имеет значение, зависящее от реализации, меньшее или равное максимальному `high` пределу.

Установка `high=0` также приводит к автоматическому выставлению `low=0` и вызывает вызов метод [`protocol.pause_writing()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseProtocol.pause_writing) всякий раз, когда буфер становится непустым.

Установка `low=0` приводит к тому, что [`protocol.resume_writing()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseProtocol.resume_writing) вызывается только после того, как буфер пуст. Использование нуля для любого предела обычно неоптимально, так как уменьшает возможности одновременного выполнения операций ввода-вывода и вычислений.

Используйте метод [`Transport.get_write_buffer_limits()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#WriteTransport.get_write_buffer_limits), чтобы получить эти ограничения.

##### _`WriteTransport.write(data)`_:

Метод `WriteTransport.write()` записывает несколько байтов данных в транспорт.

Этот метод не блокирует, он буферизует данные и организует их асинхронную отправку.

##### _`WriteTransport.writelines(list_of_data)`_:

Метод `WriteTransport.writelines()` записывает список (или итерируемую последовательность) байтов данных в транспорт. Это функционально эквивалентно вызову [`Transport.write()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#WriteTransport.write) для каждого элемента, полученного итерацией, но может быть реализовано более эффективно.

##### _`WriteTransport.write_eof()`_:

Метод `WriteTransport.write_eof()` закрывает записывающую сторону транспорта, после очистки всех буферизованных данных. Данные все еще могут быть получены.

Этот метод может вызвать [исключение `NotImplementedError`](https://docs-python.ru/tutorial/vstroennye-iskljuchenija-interpretator-python/vstroennye-iskljuchenija/ "Исключения наследуемые от Exception в Python."), если транспорт (например, SSL) не поддерживает полузакрытые соединения.

### _`asyncio.ReadTransport(BaseTransport)`_:

Класс `asyncio.ReadTransport()` представляет собой базовый транспорт для подключений, только для чтения.

Экземпляры класса `ReadTransport` возвращаются из метода цикла событий [`loop.connect_read_pipe()`](https://docs-python.ru/standart-library/modul-asyncio-python/zapusk-podprotsessov-tsikla-sobytij-asyncio/ "Создание субпроцесса из цикла событий asyncio в Python."), а также используются методами, связанными с подпроцессами, такими как [`loop.subprocess_exec()`](https://docs-python.ru/standart-library/modul-asyncio-python/zapusk-podprotsessov-tsikla-sobytij-asyncio/ "Создание субпроцесса из цикла событий asyncio в Python.").

- `ReadTransport.is_reading()` [проверяет, получает ли новые данные](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#ReadTransport.is_reading),
- `ReadTransport.pause_reading()` [приостанавливает читающую сторону транспорта](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#ReadTransport.pause_reading),
- `ReadTransport.resume_reading()` [возобновляет читающую сторону транспорта](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#ReadTransport.resume_reading),

##### _`ReadTransport.is_reading()`_:

Метод `ReadTransport.is_reading()` возвращает `True`, если транспорт получает новые данные.

> Новое в Python 3.7.

##### _`ReadTransport.pause_reading()`_:

Метод `ReadTransport.pause_reading()` приостанавливает читающую сторону транспорта. Никакие данные не будут передаваться в метод протокола [`Protocol.data_received()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#Protocol.data_received) до тех пор, пока не будет вызван [`Protocol.resume_reading()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#ReadTransport.resume_reading).

_Изменено в Python 3.7_: метод идемпотентен, т.е. его можно вызывать, когда транспорт уже приостановлен или закрыт.

##### _`ReadTransport.resume_reading()`_:

Метод `ReadTransport.resume_reading()` возобновляет читающую сторону транспорта. Метод протокола [`Protocol.data_received()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#Protocol.data_received) будет вызван еще раз, если какие-то данные стали доступны для чтения.

_Изменено в Python 3.7_: метод идемпотентен, т.е. его можно вызывать, когда транспорт уже что-то читает.

### _`asyncio.Transport(WriteTransport, ReadTransport)`_:

Класс `asyncio.Transport()` представляет собой интерфейс, представляющий двунаправленный транспорт, например TCP-соединение.

Пользователь не должен создавать экземпляр транспорта напрямую. Пользователи должны вызывать служебную функцию, передавая ей фабрику протокола и другую информацию, необходимую для создания транспорта и протокола.

Экземпляры класса `Transport` возвращаются или используются такими методами цикла событий, как [`loop.create_connection()`, `loop.create_unix_connection()`](https://docs-python.ru/standart-library/modul-asyncio-python/sozdanie-tcp-udp-unix-soedinenij-modulem-asyncio/ "Создание TCP, UDP и Unix соединений из цикла событий asyncio в Python."), [`loop.create_server()`](https://docs-python.ru/standart-library/modul-asyncio-python/sozdanie-setevyh-serverov-pomoschi-modulja-asyncio/ "Создание сетевых серверов из цикла событий asyncio в Python."), [`loop.sendfile()`](https://docs-python.ru/standart-library/modul-asyncio-python/peredacha-fajlov-tsikla-sobytij-asyncio/ "Передача файлов из цикла событий asyncio в Python.") и т. Д.

### _`asyncio.DatagramTransport(BaseTransport)`_:

Класс `asyncio.DatagramTransport()` представляет собой транспорт для датаграммных (UDP) соединений.

Экземпляры класса `DatagramTransport` возвращаются из метода цикла событий [`loop.create_datagram_endpoint()`](https://docs-python.ru/standart-library/modul-asyncio-python/sozdanie-tcp-udp-unix-soedinenij-modulem-asyncio/ "Создание TCP, UDP и Unix соединений из цикла событий asyncio в Python.").

- `DatagramTransport.sendto()` [отправляет байты данных](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#DatagramTransport.sendto),
- `DatagramTransport.abort()` [немедленно закрывает транспорт](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#DatagramTransport.abort).

##### _`DatagramTransport.sendto(data, addr=None)`_:

Метод `DatagramTransport.sendto()` отправляет байты данных `data` удаленному хосту, заданному аргументом `addr` (целевой адрес, зависящий от транспорта). Если `addr=None`, то данные отправляются на целевой адрес, указанный при создании транспорта.

Этот метод не блокирует, он буферизует данные и организует их асинхронную отправку.

> Изменено в Python 3.13: Этот метод может вызываться с объектом `empty bytes` для отправки дейтаграммы нулевой длины. Расчет размера буфера, используемый для управления потоком, также обновлен для учета заголовка дейтаграммы.

##### _`DatagramTransport.abort()`_:

Метод `DatagramTransport.abort()` немедленно закрывает транспорт, не дожидаясь завершения незавершенных операций. Буферизованные данные будут потеряны. Больше никаких данных не будет.

В конечном итоге метод протокола [`protocol.connection_lost()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseProtocol.connection_lost) будет вызываться с аргументом `None`.

### _`asyncio.SubprocessTransport(BaseTransport)`_:

Абстрактный класс `asyncio.SubprocessTransport()` представляет связь между родительским и дочерним процессом ОС.

Экземпляры класса `SubprocessTransport` возвращаются из методов цикла событий [`loop.subprocess_shell()` и `loop.subprocess_exec()`](https://docs-python.ru/standart-library/modul-asyncio-python/zapusk-podprotsessov-tsikla-sobytij-asyncio/ "Создание субпроцесса из цикла событий asyncio в Python.").

Подробнее о предоставляемых методах смотрите в разделе "[Создание субпроцесса из цикла событий asyncio](https://docs-python.ru/standart-library/modul-asyncio-python/zapusk-podprotsessov-tsikla-sobytij-asyncio/ "Создание субпроцесса из цикла событий asyncio в Python.")".

---

## Протоколы.

Модуль `asyncio` предоставляет набор абстрактных базовых классов, которые следует использовать для реализации сетевых протоколов. Эти классы предназначены для использования вместе с [транспортами](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#Transport).

Подклассы абстрактных базовых классов протокола могут реализовывать некоторые или все методы. Все эти методы являются обратными вызовами: они вызываются транспортом при определенных событиях, например, при получении некоторых данных. Метод базового протокола должен вызываться соответствующим транспортом.

### _`asyncio.BaseProtocol`_:

Атрибут `asyncio.BaseProtocol` представляет собой базовый протокол с методами, общими для всех протоколов.

- Обратные вызовы соединения:
    - `BaseProtocol.connection_made()` [вызывается при установлении соединения](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseProtocol.connection_made),
    - `BaseProtocol.connection_lost()` [вызывается, когда соединение потеряно](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseProtocol.connection_lost),
- Обратные вызовы управления потоком:
    - `BaseProtocol.pause_writing()` [вызывается, когда буфер больше предела `high`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseProtocol.pause_writing),
    - `BaseProtocol.resume_writing()` [вызывается, когда буфер меньше предела `low`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseProtocol.resume_writing),

#### Обратные вызовы соединения.

Обратные вызовы соединения вызываются по всем протоколам ровно один раз за успешное соединение. Все остальные обратные вызовы протокола могут быть вызваны только между этими двумя методами.

##### _`BaseProtocol.connection_made(transport)`_:

Метод `BaseProtocol.connection_made()` вызывается при установлении соединения.

Аргумент `transport` - это транспорт, представляющий соединение. Протокол отвечает за хранение ссылки на свой транспорт.

##### _`BaseProtocol.connection_lost(exc)`_:

Метод `BaseProtocol.connection_lost()` вызывается, когда соединение потеряно или закрыто.

Аргумент `exc` является либо объектом исключения, либо `None`. Последнее означает, что получен обычный EOF, или соединение было прервано или закрыто этой стороной соединения.

#### Обратные вызовы управления потоком.

Обратные вызовы управления потоком могут быть вызваны транспортом для приостановки или возобновления записи, выполняемой протоколом.

Смотрите документацию метода [`Transport.set_write_buffer_limits()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#WriteTransport.set_write_buffer_limits) для получения более подробной информации.

##### _`BaseProtocol.pause_writing()`_:

Метод `BaseProtocol.pause_writing()` вызывается, когда буфер транспорта выходит за верхний разрешенный предел.

##### _`BaseProtocol.resume_writing()`_:

Метод `BaseProtocol.resume_writing()` вызывается, когда буфер транспорта опускается ниже нижнего разрешенного предела.

Если размер буфера равен верхнему значению предела, то метод [`Protocol.pause_writing()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseProtocol.pause_writing) не вызывается: размер буфера должен строго превышать.

И наоборот, [`Protocol.resume_writing()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BaseProtocol.resume_writing) вызывается, когда размер буфера равен или меньше нижнего разрешенного предела. Эти конечные условия важны для обеспечения того, чтобы все шло так, как ожидалось, когда любая из оценок равна нулю.

### _`asyncio.Protocol(BaseProtocol)`_:

Класс `asyncio.Protocol()` представляет собой базовый класс для реализации потоковых протоколов (TCP, Unix sockets и т. д.).

Все протоколы `asyncio` могут реализовывать обратные вызовы базового протокола.

Методы событий, такие как `loop.create_server()`, `loop.create_unix_server()`, `loop.create_connection()`, `loop.create_unix_connection()`, `loop.connect_accepted_socket()`, `loop.connect_read_pipe()` и `loop.connect_write_pipe()`, принимают фабрики, которые возвращают протоколы потоковой передачи.

- `Protocol.data_received()` [вызывается при получении данных](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#Protocol.data_received),
- `Protocol.eof_received()` [вызывается, когда другой хост больше не будет отправлять данные](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#Protocol.eof_received),

##### _`Protocol.data_received(data)`_:

Метод `Protocol.data_received()` вызывается при получении данных.

Аргумент `data` - это непустой байтовый объект, содержащий входящие данные.

Будут ли данные буферизированы, разделены на части или повторно собраны, зависит от транспорта. В общем, не надо полагаться на конкретную семантику и делайте синтаксический анализ универсальным и гибким. Данные всегда поступают в правильном порядке.

Метод можно вызывать произвольное количество раз, пока открыто соединение.

НО метод [`Protocol.eof_received()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#Protocol.eof_received) вызывается не более одного раза. После вызова [`Protocol.eof_received()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#Protocol.eof_received), метод `Protocol.data_received()` больше не вызывается.

##### _`Protocol.eof_received()`_:

Метод `Protocol.eof_received()` вызывается, когда на другом конце сообщают, что больше не будет отправлять данные например, путем вызова [`Transport.write_eof()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#WriteTransport.write_eof), если конечно другой конец также использует `asyncio`.

Этот метод может вернуть ложное значение (включая `None`), и в этом случае транспорт закроется. И наоборот, если этот метод возвращает истинное значение, то используемый протокол определяет, следует ли закрыть транспорт. Так как реализация по умолчанию возвращает `None`, то она неявно закрывает соединение.

Некоторые транспорты, включая SSL, не поддерживают полузакрытые соединения, и в этом случае возврат значения `True` из этого метода приведет к закрытию соединения.

Механизм состояний:

start -> connection_made
    [-> data_received]*
    [-> eof_received]?
-> connection_lost -> end

### _`asyncio.BufferedProtocol(BaseProtocol)`_:

Класс `asyncio.BufferedProtocol()` представляет собой базовый класс для реализации потоковых протоколов с ручным управлением приемным буфером.

Новое в Python 3.7.

Буферизованные протоколы можно использовать с любым методом цикла событий, который поддерживает протоколы потоковой передачи.

Реализации `BufferedProtocol` допускают явное ручное выделение буфера приема и управление им. Затем, циклы событий могут использовать буфер, предоставленный протоколом, чтобы избежать ненужных копий данных. Это может привести к заметному повышению производительности протоколов, которые получают большие объемы данных. Сложные реализации протокола могут значительно сократить количество выделяемых буферов.

Для экземпляров `BufferedProtocol` вызываются следующие обратные вызовы:

- `BufferedProtocol.get_buffer()` [вызывается для выделения нового буфера](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BufferedProtocol.get_buffer),
- `BufferedProtocol.buffer_updated()` [вызывается, когда буфер был обновлен](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BufferedProtocol.buffer_updated),
- `BufferedProtocol.eof_received()` [вызывается, когда другой хост больше не будет отправлять данные](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BufferedProtocol.eof_received).

##### _`BufferedProtocol.get_buffer(sizehint)`_:

Метод `BufferedProtocol.get_buffer()` вызывается для выделения нового буфера приема.

Аргумент `sizehint` - рекомендуемый минимальный размер возвращаемого буфера. Допустимо использовать буферы меньшего или большего размера, чем предлагает `sizehint`. При значении `sizehint=-1` размер буфера может быть произвольным. Использование буфера нулевого размера является ошибкой.

Метод `BufferedProtocol.get_buffer()` должен возвращать объект, реализующий протокол буфера.

##### _`BufferedProtocol.buffer_updated(nbytes)`_:

Метод `BufferedProtocol.buffer_updated()` вызывается, когда буфер был обновлен полученными данными.

Аргумент `nbytes` - это общее количество байтов, записанных в буфер.

##### _`BufferedProtocol.eof_received()`_:

Метод `BufferedProtocol.eof_received()` смотрите документацию метода [`Protocol.eof_received()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#Protocol.eof_received).

[`Protocol.get_buffer()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BufferedProtocol.get_buffer) можно вызывать произвольное количество раз во время соединения. Однако `protocol.eof_received()` вызывается не более одного раза, и если вызывается, то методы [`Protocol.get_buffer()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BufferedProtocol.get_buffer) и [`Protocol.buffer_updated()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#BufferedProtocol.buffer_updated) не будут вызываться после него.

Механизм состояний:

start -> connection_made
    [-> get_buffer
        [-> buffer_updated]?
    ]*
    [-> eof_received]?
-> connection_lost -> end

### _`asyncio.DatagramProtocol(BaseProtocol)`_:

Класс `asyncio.DatagramProtocol()` представляет собой базовый класс для реализации протоколов дейтаграмм (UDP).

Экземпляры UDP протокола должны быть построены фабриками протоколов, переданными в метод [`loop.create_datagram_endpoint()`](https://docs-python.ru/standart-library/modul-asyncio-python/sozdanie-tcp-udp-unix-soedinenij-modulem-asyncio/ "Создание TCP, UDP и Unix соединений из цикла событий asyncio в Python.").

- `DatagramProtocol.datagram_received()` [Метод](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#DatagramProtocol.datagram_received),
- `DatagramProtocol.error_received()` [Метод](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#DatagramProtocol.error_received),

##### _`DatagramProtocol.datagram_received(data, addr)`_:

Метод `DatagramProtocol.datagram_received()` вызывается при получении UDP соединением входящих данных.

- `data` - это байтовый объект, содержащий входящие данные.
- `addr` - адрес хоста, отправляющего данные, точный формат зависит от транспорта.

##### _`DatagramProtocol.error_received(exc)`_:

Метод `DatagramProtocol.error_received()` вызывается, когда предыдущая операция отправки или получения вызывает [ошибку `OSError`](https://docs-python.ru/tutorial/vstroennye-iskljuchenija-interpretator-python/oshibki-operatsionnoj-sistemy-oserror/ "Исключения операционной системы: OSError в Python.").

Аргумент `exc` - это экземпляр `OSError`.

Этот метод вызывается в редких случаях, когда UDP транспорт обнаруживает, что дейтаграмма не может быть доставлена ​​получателю. Во многих случаях недоставленные дейтаграммы автоматически отбрасываются.

_Примечание_. В системах BSD (macOS, FreeBSD и т. д.) управление потоком не поддерживается для протоколов дейтаграмм, т.к. нет надежного способа обнаружить сбоя отправки, вызванные записью слишком большого количества пакетов.

Сокет всегда выглядит "_готовым_", и лишние пакеты отбрасываются. Исключение `OSError` с `errno`, установленным в `errno.ENOBUFS` может быть вызвано или нет. Если исключение будет вызвано, то будет сообщен [`DatagramProtocol.error_received()`](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#DatagramProtocol.error_received), но в противном случае проигнорирован.

### _`asyncio.SubprocessProtocol(BaseProtocol)`_:

Класс `asyncio.SubprocessProtocol()` представляет собой базовый класс для реализации протоколов, взаимодействующих с дочерними процессами (однонаправленные каналы).

Подробнее о предоставляемых методах смотрите в разделе "[Создание субпроцесса из цикла событий asyncio](https://docs-python.ru/standart-library/modul-asyncio-python/zapusk-podprotsessov-tsikla-sobytij-asyncio/ "Создание субпроцесса из цикла событий asyncio в Python.")".

---

## Примеры использования `Transport` и `Protocol` модуля `asyncio`.

- [Пример UDP эхо-сервера](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#UDP-server),
- [Пример UDP эхо-клиента](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#UDP-client),
- [Пример подключения к существующему сокету](https://docs-python.ru/standart-library/modul-asyncio-python/transport-protokoly-modulja-asyncio/#socket).

---

### Пример UDP эхо-сервера.

UDP эхо-сервер используя метод `loop.create_datagram_endpoint()`, отправляет обратно полученные данные:

import asyncio

class EchoServerProtocol:
    def connection_made(self, transport):
        self.transport = transport

    def datagram_received(self, data, addr):
        message = data.decode()
        print('Received %r from %s' % (message, addr))
        print('Send %r to %s' % (message, addr))
        self.transport.sendto(data, addr)

async def main():
    print("Starting UDP server")

    # Получаем ссылку на цикл событий, т.к. планируем
    # использовать низкоуровневый API.
    loop = asyncio.get_running_loop()

    # Для обслуживания всех клиентских запросов 
    # будет создан один экземпляр протокола.
    transport, protocol = await loop.create_datagram_endpoint(
        lambda: EchoServerProtocol(),
        local_addr=('127.0.0.1', 9999))

    try:
        await asyncio.sleep(3600)  # Serve for 1 hour.
    finally:
        transport.close()

asyncio.run(main())

---

### Пример UDP эхо-клиента.

UDP эхо-клиент, используя метод `loop.create_datagram_endpoint()`, отправляет данные, а когда получает ответ - закрывает транспорт:

import asyncio

class EchoClientProtocol:
    def __init__(self, message, on_con_lost):
        self.message = message
        self.on_con_lost = on_con_lost
        self.transport = None

    def connection_made(self, transport):
        self.transport = transport
        print('Send:', self.message)
        self.transport.sendto(self.message.encode())

    def datagram_received(self, data, addr):
        print("Received:", data.decode())

        print("Close the socket")
        self.transport.close()

    def error_received(self, exc):
        print('Error received:', exc)

    def connection_lost(self, exc):
        print("Connection closed")
        self.on_con_lost.set_result(True)

async def main():
    # Получаем ссылку на цикл событий, т.к. 
    # планируем использовать низкоуровневый API.
    loop = asyncio.get_running_loop()

    on_con_lost = loop.create_future()
    message = "Hello World!"

    transport, protocol = await loop.create_datagram_endpoint(
        lambda: EchoClientProtocol(message, on_con_lost),
        remote_addr=('127.0.0.1', 9999))

    try:
        await on_con_lost
    finally:
        transport.close()

asyncio.run(main())

---

### Пример подключения к существующему сокету.

В примере будем ждать, пока сокет получит данные, используя метод `loop.create_connection()` с протоколом:

import asyncio
import socket

class MyProtocol(asyncio.Protocol):

    def __init__(self, on_con_lost):
        self.transport = None
        self.on_con_lost = on_con_lost

    def connection_made(self, transport):
        self.transport = transport

    def data_received(self, data):
        print("Received:", data.decode())

        # Готово: закрываем транспорт;
        # connection_lost() будет вызываться автоматически.
        self.transport.close()

    def connection_lost(self, exc):
        # The socket has been closed
        self.on_con_lost.set_result(True)

async def main():
    # Получаем ссылку на цикл событий, т.к. 
    # планируем использовать низкоуровневый API.
    loop = asyncio.get_running_loop()
    on_con_lost = loop.create_future()

    # Создаем пару подключенных сокетов
    rsock, wsock = socket.socketpair()

    # Регистрируем сокет для получения данных
    transport, protocol = await loop.create_connection(
        lambda: MyProtocol(on_con_lost), sock=rsock)

    # Имитация приема данных из сети.
    loop.call_soon(wsock.send, 'abc'.encode())

    try:
        await protocol.on_con_lost
    finally:
        transport.close()
        wsock.close()

asyncio.run(main())

Смотрите пример слежения за файловым дескриптором на предмет событий чтения в разделе "[Наблюдение за дескрипторами файлов из цикла событий](https://docs-python.ru/standart-library/modul-asyncio-python/nabljudenie-deskriptorami-fajlov-tsikla-sobytij-asyncio/ "Наблюдение за дескрипторами файлов из цикла событий asyncio в Python.")", в нем используется низкоуровневый метод `loop.add_reader()` для регистрации `fd`.

Также смотрите пример регистрации открытого сокета для ожидания данных с использованием потоков `stream`. Пример использует высокоуровневый API [потоков `stream` модуля `asyncio`](https://docs-python.ru/standart-library/modul-asyncio-python/potoki-stream-modulja-asyncio/ "Работа с сетевыми соединениями модуля asyncio в Python."), созданные функцией `asyncio.open_connection()` в сопрограмме.