# [Bluetooth LE](https://blog.stylingandroid.com/bluetooth-le-part-1/)

作者：Mark Allison

日期：2014年4月11日

Bluetooth LE is a technology that is going to be central to not only wearable technology, but to many IoT devices.

Bluetooth has been around since the mid-to-late 1990's and has become the standard for peer-to-peer networking of devices over short distances. 缺点是耗电。并且配对后才能通信，配对体验较差。

Bluetooth Low Energy, or Bluetooth LE for short, or BLE for even shorter still, was introduced as part of the Bluetooth 4.0 (sometimes called Bluetooth Smart) specification, and addresses these specific issues. Google added BLE support to Android 4.3 (API 18).

Let's take a brief look at the main differences between traditional Bluetooth development and BLE.

The first major difference is in the pairing process. With BLE that responsibility lies much more with the developer.

The other major difference is the actual communication itself. Traditional Bluetooth communication were based on a socket architecture very similar to standard network sockets. BLE centres around attributes. An attribute is an atomic piece of data (i.e. an integer or string) which is shared between both devices. Attributes may be used to either represent data or control how the sensor behaves.

Typically we will be connecting sensors(heart rate monitor, temperature sensor, etc.) to a host (smart phone, tablet, etc.)