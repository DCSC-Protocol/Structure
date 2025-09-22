# Protocol-Structure
- Основан на UDP (В частности для простого проброса P2P через UDP hole punching.)


## Root:
```
[ Default Packet Params - 1 bytes ] [ Packet Type - 1 bytes ] [ Signded Hash Len (uint16) - 2 bytes ] [ Signded Hash - ~ bytes ] [ Timestamp (uint64) - 8 bytes ] [ Payload ]
```

- __Default Packet Params__

  Байт делится на 8 бит (литерали bool) определяющих параметры:

  __[0] - Who sender?__ _true_ - Пакет отправлен сервером (Пиром), _false_ - Пакет отправлен клиентом или P2P стороной

  __[1] - Who receiver?__ _true_ - Пакет предназначен серверу (Пиру), _false_ - Пакет предназначен клиенту или P2P стороне

  __[2] - Use shifts?__ _true_ - Пакет использует сдвиги заголовков, предназначается для обхода DPI, в таком случае структура пакета становится такой:
  ```
  [ Default Packet Params - 1 bytes ] [ Shift len - 1 bytes ] [ Packet Type - 1 bytes ] [ Shift - ~ bytes] [ Signded Hash Len (uint16) - 2 bytes ] [ Shift - ~ bytes] [ Signded Hash - ~ bytes ] [ Shift - ~ bytes] [ Timestamp (uint64) - 8 bytes ] [ Shift - ~ bytes] [ Payload ]
  ```
  __Shift len__ - указывает количество пропущенных байт между полями (Максимум 256), пустые участки '__Shift__' можно забить любым мусором.

  Если же _false_ - Пакет использует стандартный формат.
  
- __Pakcet Type__

  Число от 0 до 255.

  _0 - Всегда keepalive, не требуется ответ на него._

  Остальные коды зависят от контекста.

- __Signed Hash Len__

  Длина подписи.

- __Signed Hash__
  
  Подпись пакета, по сути хеш Timestamp + Payload зашифрованный приватным ключом клиента.
 
- __Timestamp__

  Обычная юниксовская UTC таймштампа, нужна по сути только от replay атак.
