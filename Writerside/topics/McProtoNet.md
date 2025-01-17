# McProtoNet

В этой библиотеке собраны самые основные классы, которые 
помогают работать с протоколом Minecraft.
Библиотека включает инструменты для чтения и записи пакетов, 
обработки сетевых потоков, а также шифрования и сжатия данных. 

## Работа с пакетами

В пространстве имен `McProtoNet.Net` расположены основные классы, 
которые обеспечивают работу с пакетами. Они читают и записывают
пакеты согласно [протоколу Minecraft](https://minecraft.wiki/w/Minecraft_Wiki:Projects/wiki.vg_merge/Protocol#Packet_format).

1. `MinecraftPacketReader`

Класс `MinecraftPacketReader` предназначен для чтения пакетов из любого потока, в том числе, сетевого.

Для этого есть метод `ReadNextPacketAsync`, который возвращает [](McProtoNet-Abstractions.md#inputpacket).

<code-block lang="C#" src="../code-samples/ReadPacketSample.cs"/>

2. `MinecraftPacketSender`

Этот класс занимается отправкой пакетов. Добиться этого можно с помощью метода `SendPacketAsync`,
который в качестве аргумента принимает `ReadOnlyMemory<byte>`. 

`ReadOnlyMemory<byte>` - это отправленный пакет, у которого первые байты это идентификатор, остальное - данные.

```C#
NetworkStream ns = new NetworkStream(socket);

MinecraftPacketSender sender = new MinecraftPacketSender();
sender.BaseStream = ns;
byte[] data = [0, 3, 4, 5, 6];
await sender.SendPacketAsync(data, cancellationToken: default(CancellationToken));
```

Также имеются методы расширения для [](McProtoNet-Abstractions.md#outputpacket). Это удобно, если
Вы отправляете пакет прямиком из `MinecraftPrimitiveWriter`.

### Сжатие

Оба, вышеупомянутых класса, позволяют включить сжатие.
Это достигается с помощью метода `SwitchCompression`, который принимает
в качестве аргумента число — порог сжатия.

Для повышения производительности при сжатии и 
декомпрессии пакетов используется библиотека [libdeflate](https://github.com/ebiggers/libdeflate).

## Криптография

Пространство имён `McProtoNet.Cryptography` содержит два класса для упрощения
работы с шифрованием:

1. `CryptoHandler`

Утилитарный класс, который предоставляет некоторые методы:

<deflist>
<def title="DecodeRSAPublicKey">
<p>

</p>
</def>
<def title="GenerateAESPrivateKey(string,string, byte[])">
<p>
Генерирует случайный 128-битный приватный ключ.
</p>
<p>
Параметры:
</p>
<p>
<b>serverID</b> - 
</p>
<p>
Возвращает <code>byte[]</code> - ключ.
</p>
</def>
<def title="GetServerHash">
<p>

</p>
</def>
</deflist>


## Класс `MinecraftClient`

Пространство имён `McProtoNet.Client` содержит класс `MinecraftClient`, который