# gg1

## Идентификаторы пакетов

Допустим мы сериализовали пакет и теперь его надо отправить, но для этого, согласно протоколу Minecraft,
нужно записать в поток идентификатор, чтобы сервер мог понять какой пакет к нему пришел.
В McProtoNet это реализовано с помощью большого словаря, где ключ состоит из версии протокола
и названия пакета, а значение - идентификатор данного пакета и версии. Это было достигнуто за счет кодогенерации.