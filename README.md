# Protocol-Structure
- Основан на UDP (В частности для простого проброса P2P через UDP hole punching.)



## Root:
```
[ Default Packet Params - 1 byte ] [ Packet Type 1 byte ] [ Payload ]
```
### Default Packet Params

Байт делится на 8 бит (литерали bool) определяющих параметры:

__[0] - Who sender?__ _true_ - Пакет отправлен сервером (Пиром), _false_ - Пакет отправлен клиентом или P2P стороной

__[1] - Who receiver?__ _true_ - Пакет предназначен серверу (Пиру), _false_ - Пакет предназначен клиенту или P2P стороне

### Pakcet Type

Число от 0 до 255, _0 - Всегда keepalive_, по сути простой пинг, не требуется ответ на него. Остальные коды зависят от контекста.
