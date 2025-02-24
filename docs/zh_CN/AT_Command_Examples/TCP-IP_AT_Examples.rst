TCP-IP AT 示例
==================

:link_to_translation:`en:[English]`

.. contents::
   :local:
   :depth: 1

ESP 设备作为 TCP 客户端建立单连接
--------------------------------------------

#. 设置 Wi-Fi 模式为 Station。

   命令：

   .. code-block:: none

     AT+CWMODE=1

   响应：

   .. code-block:: none

     OK

#. 连接到路由器。

   命令：

   .. code-block:: none

     AT+CWJAP="espressif","1234567890"

   响应：

   .. code-block:: none

     WIFI CONNECTED
     WIFI GOT IP

     OK

   说明：

   - 您输入的 SSID 和密码可能跟上述命令中的不同。请使用您的路由器的 SSID 和密码。

#. 查询 ESP 设备 IP 地址。

   命令：

   .. code-block:: none

     AT+CIPSTA?

   响应：

   .. code-block:: none

    +CIPSTA:ip:"192.168.3.112"
    +CIPSTA:gateway:"192.168.3.1"
    +CIPSTA:netmask:"255.255.255.0"

    OK

   说明：

   - 您的查询结果可能与上述响应中的不同。

#. PC 与 ESP 设备连接同一个路由。

   在 PC 上使用网络调试工具，创建一个 TCP 服务器。例如 TCP 服务器的 IP 地址为 ``192.168.3.102``，端口为 ``8080``。

#. ESP 设备作为客户端通过 TCP 连接到 TCP 服务器，服务器 IP 地址为 ``192.168.3.102``，端口为 ``8080``。

   命令：

   .. code-block:: none

     AT+CIPSTART="TCP","192.168.3.102",8080

   响应：

   .. code-block:: none

     CONNECT

     OK

#. 发送 4 字节数据。

   命令：

   .. code-block:: none

     AT+CIPSEND=4

   响应：

   .. code-block:: none

     OK

     >

   输入 4 字节数据，例如输入数据是 ``test``，之后 AT 将会输出以下信息。

   .. code-block:: none

     Recv 4 bytes

     SEND OK

   说明：

   - 若输入的字节数目超过 ``AT+CIPSEND`` 命令设定的长度 (n)，则系统会响应 ``busy p...``，并发送数据的前 n 个字节，发送完成后响应 ``SEND OK``。

#. 接收 4 字节数据。

   假设 TCP 服务器发送 4 字节的数据（数据为 ``test``），则系统会提示：

   .. code-block:: none

     +IPD,4:test

ESP 设备作为 TCP 服务器建立多连接
--------------------------------------------

当 ESP 设备作为 TCP 服务器时，必须使能多连接，即允许多个 TCP 客户端连接到 ESP 设备创建的服务器。

以下是 ESP 设备作为 SoftAP 建立 TCP 服务器的示例；如果是 ESP 设备作为 station，可在连接路由器后按照同样方法建立服务器。

#. 设置 Wi-Fi 模式为 SoftAP。

   命令：

   .. code-block:: none

     AT+CWMODE=2

   响应：

   .. code-block:: none

     OK

#. 使能多连接。

   命令：

   .. code-block:: none

     AT+CIPMUX=1

   响应：

   .. code-block:: none

     OK

#. 设置 SoftAP。

   命令：

   .. code-block:: none

     AT+CWSAP="ESP32_SOFTAP","1234567890",5,3

   响应：

   .. code-block:: none

     OK

#. 查询 SoftAP 信息。

   命令：

   .. code-block:: none

     AT+CIPAP?

   响应：

   .. code-block:: none

     AT+CIPAP?
     +CIPAP:ip:"192.168.4.1"
     +CIPAP:gateway:"192.168.4.1"
     +CIPAP:netmask:"255.255.255.0"

     OK

   说明：

   - 您查询到的地址可能与上述响应中的不同。

#. 建立 TCP 服务器，默认端口为 ``333``。

   命令：

   .. code-block:: none

     AT+CIPSERVER=1

   响应：

   .. code-block:: none

     OK

#. PC 连接到 ESP 设备的 SoftAP。

   .. figure:: ../../img/Connect-SoftAP.png
       :scale: 100 %
       :align: center
       :alt: Connect SoftAP

#. 在 PC 上使用网络调试工具创建一个 TCP 客户端，连接到 ESP 设备创建的 TCP 服务器。

#. 发送 4 字节数据到网络连接 ID 为 0 的链路上。

   命令：

   .. code-block:: none

     AT+CIPSEND=0,4

   响应：

   .. code-block:: none

     OK

     >

   输入 4 字节数据，例如输入数据是 ``test``，之后 AT 将会输出以下信息。

   .. code-block:: none

     Recv 4 bytes

     SEND OK

   说明：

   - 若输入的字节数目超过 ``AT+CIPSEND`` 命令设定的长度 (n)，则系统会响应 ``busy p...``，并发送数据的前 n 个字节，发送完成后响应 ``SEND OK``。

#. 从网络连接 ID 为 0 的链路上接收 4 字节数据。

   假设 TCP 服务器发送 4 字节的数据（数据为 ``test``），则系统会提示：

   .. code-block:: none

     +IPD,0,4:test

#. 关闭 TCP 连接。

   命令：

   .. code-block:: none

     AT+CIPCLOSE=0

   响应：

   .. code-block:: none

     0,CLOSED

     OK

远端 IP 地址和端口固定的 UDP 通信
-------------------------------------------------

#. 设置 Wi-Fi 模式为 Station。

   命令：

   .. code-block:: none

     AT+CWMODE=1

   响应：

   .. code-block:: none

     OK

#. 连接到路由器。

   命令：

   .. code-block:: none

     AT+CWJAP="espressif","1234567890"

   响应：

   .. code-block:: none

     WIFI CONNECTED
     WIFI GOT IP

     OK

   说明：

   - 您输入的 SSID 和密码可能跟上述命令中的不同。请使用您的路由器的 SSID 和密码。

#. 查询 ESP 设备 IP 地址。

   命令：

   .. code-block:: none

     AT+CIPSTA?

   响应：

   .. code-block:: none

    +CIPSTA:ip:"192.168.3.112"
    +CIPSTA:gateway:"192.168.3.1"
    +CIPSTA:netmask:"255.255.255.0"

    OK

   说明：

   - 您的查询结果可能与上述响应中的不同。

#. PC 与 ESP 设备连接到同一个路由。

   在 PC 上使用网络调试工具，创建一个 UDP 传输。例如 PC 的 IP 地址为 ``192.168.3.102``，端口为 ``8080``。

#. 使能多连接。

   命令：

   .. code-block:: none

     AT+CIPMUX=1

   响应：

   .. code-block:: none

     OK

#. 创建 UDP 传输。分配网络连接 ID 为 4，远程 IP 地址为 ``192.168.3.102``，远端端口为 ``8080``，本地端口为 ``1112``，模式为 ``0``。

   .. Important::

     ``AT+CIPSTART`` 命令的参数 ``mode`` 决定了 UDP 通信的远端 IP 地址和端口是否固定。若参数为 0，则代表系统会分配一个特定网络连接 ID，以确保通信过程中远端的 IP 地址和端口不会被改变，且数据发送端和数据接收端不会被其它设备代替。

   命令：

   .. code-block:: none

     AT+CIPSTART=4,"UDP","192.168.3.102",8080,1112,0

   响应：

   .. code-block:: none

     4,CONNECT

     OK

   说明：

   - ``"192.168.3.102"`` 和 ``8080`` 为 UDP 传输的远端 IP 地址和远端端口，也就是 PC 建立的 UDP 配置。
   - ``1112`` 为 ESP 设备的 UDP 本地端口，您可自行设置，如不设置则为随机值。
   - ``0`` 表示 UDP 远端 IP 地址和端口是固定的，不能更改。比如有另外一台 PC 创建了 UDP 端并且向 ESP 设备端口 1112 发送数据，ESP 设备仍然会接收来自 UDP 端口 1112 的数据，如果使用 AT 命令 ``AT+CIPSEND=4,X``，那么数据仍然只会发送到第一台 PC 端。但是如果这个参数未设置为 ``0``，那么数据将会被发送到新的 PC 端。

#. 发送 7 字节数据到网络连接 ID 为 4 的链路上。

   命令：

   .. code-block:: none

     AT+CIPSEND=4,7

   响应：

   .. code-block:: none

     OK

     >

   输入 7 字节数据，例如输入数据是 ``abcdefg``，之后 AT 将会输出以下信息。

   .. code-block:: none

     Recv 7 bytes

     SEND OK

   说明：

   - 若输入的字节数目超过 ``AT+CIPSEND`` 命令设定的长度 (n)，则系统会响应 ``busy p...``，并发送数据的前 n 个字节，发送完成后响应 ``SEND OK``。

#. 从网络连接 ID 为 4 的链路上接收 4 字节数据。

   假设 PC 发送 4 字节的数据（数据为 ``test``），则系统会提示：

   .. code-block:: none

     +IPD,4,4:test

#. 关闭网络连接 ID 为 4 的 UDP 连接。

   命令：

   .. code-block:: none

     AT+CIPCLOSE=4

   响应：

   .. code-block:: none

     4,CLOSED

     OK

远端 IP 地址和端口可变的 UDP 通信
----------------------------------------------------

#. 设置 Wi-Fi 模式为 Station。

   命令：

   .. code-block:: none

     AT+CWMODE=1

   响应：

   .. code-block:: none

     OK

#. 连接到路由器。

   命令：

   .. code-block:: none

     AT+CWJAP="espressif","1234567890"

   响应：

   .. code-block:: none

     WIFI CONNECTED
     WIFI GOT IP

     OK

   说明：

   - 您输入的 SSID 和密码可能跟上述命令中的不同。请使用您的路由器的 SSID 和密码。

#. 查询 ESP 设备 IP 地址。

   命令：

   .. code-block:: none

     AT+CIPSTA?

   响应：

   .. code-block:: none

    +CIPSTA:ip:"192.168.3.112"
    +CIPSTA:gateway:"192.168.3.1"
    +CIPSTA:netmask:"255.255.255.0"

    OK

   说明：

   - 您的查询结果可能与上述响应中的不同。

#. PC 与 ESP 设备连接到同一个路由。

   在 PC 上使用网络调试工具，创建一个 UDP 传输。例如 IP 地址为 ``192.168.3.102``，端口为 ``8080``。

#. 使能单连接。

   命令：

   .. code-block:: none

     AT+CIPMUX=0

   响应：

   .. code-block:: none

     OK

#. 创建 UDP 传输。远程 IP 地址为 ``192.168.3.102``，远端端口为 ``8080``，本地端口为 ``1112``，模式为 ``2``。

   命令：

   .. code-block:: none

     AT+CIPSTART="UDP","192.168.3.102",8080,1112,2

   响应：

   .. code-block:: none

     CONNECT

     OK

   说明：

   - ``"192.168.3.102"`` 和 `8080` 为 UDP 传输的远端 IP 地址和远端端口，也就是 PC 建立的 UDP 配置。
   - ``1112`` 为 ESP 设备的 UDP 本地端口，您可自行设置，如不设置则为随机值。
   - ``2`` 表示当前 UDP 传输建立后，UDP 传输远端信息仍然会更改；UDP 传输的远端信息会自动更改为最近一次与 ESP 设备 UDP 通信的远端 IP 地址和端口。

#. 发送 4 字节数据。

   命令：

   .. code-block:: none

     AT+CIPSEND=4

   响应：

   .. code-block:: none

     OK

     >

   输入 4 字节数据，例如输入数据是 ``test``，之后 AT 将会输出以下信息。

   .. code-block:: none

     Recv 4 bytes

     SEND OK

   说明：

   - 若输入的字节数目超过 ``AT+CIPSEND`` 命令设定的长度 (n)，则系统会响应 ``busy p...``，并发送数据的前 n 个字节，发送完成后响应 ``SEND OK``。

#. 发送 UDP 包给其它 UDP 远端。例如发送 4 字节数据，远端主机的 IP 地址为 ``192.168.3.103``，远端端口为 ``1000``。

   若需要发 UDP 包给其它 UDP 远端，只需指定对方 IP 地址和端口即可。

   命令：

   .. code-block:: none

     AT+CIPSEND=4,"192.168.3.103",1000

   响应：

   .. code-block:: none

     OK

     >

   输入 4 字节数据，例如输入数据是 ``test``，之后 AT 将会输出以下信息。

   .. code-block:: none

     Recv 4 bytes

     SEND OK

#. 接收 4 字节数据。

   假设 PC 发送 4 字节的数据（数据为 ``test``），则系统会提示：

   .. code-block:: none

     +IPD,4:test

#. 关闭 UDP 连接。

   命令：

   .. code-block:: none

     AT+CIPCLOSE

   响应：

   .. code-block:: none

     CLOSED

     OK

ESP 设备作为 TCP 客户端，建立单连接，实现 UART Wi-Fi 透传
-----------------------------------------------------------------------------------------

#. 设置 Wi-Fi 模式为 Station。

   命令：

   .. code-block:: none

     AT+CWMODE=1

   响应：

   .. code-block:: none

     OK

#. 连接到路由器。

   命令：

   .. code-block:: none

     AT+CWJAP="espressif","1234567890"

   响应：

   .. code-block:: none

     WIFI CONNECTED
     WIFI GOT IP

     OK

   说明：

   - 您输入的 SSID 和密码可能跟上述命令中的不同。请使用您的路由器的 SSID 和密码。

#. 查询 ESP 设备 IP 地址。

   命令：

   .. code-block:: none

     AT+CIPSTA?

   响应：

   .. code-block:: none

    +CIPSTA:ip:"192.168.3.112"
    +CIPSTA:gateway:"192.168.3.1"
    +CIPSTA:netmask:"255.255.255.0"

    OK

   说明：

   - 您的查询结果可能与上述响应中的不同。

#. PC 与 ESP 设备连接到同一个路由。

   在 PC 上使用网络调试工具，创建一个 TCP 服务器。例如 IP 地址为 ``192.168.3.102``，端口为 ``8080``。

#. ESP 设备作为客户端通过 TCP 连接到 TCP 服务器，服务器 IP 地址为 ``192.168.3.102``，端口为 ``8080``。

   命令：

   .. code-block:: none

     AT+CIPSTART="TCP","192.168.3.102",8080

   响应：

   .. code-block:: none

     CONNECT

     OK

#. 使能 UART Wi-Fi 透传模式。

   命令：

   .. code-block:: none

     AT+CIPMODE=1

   响应：

   .. code-block:: none

     OK

#. 在 :term:`透传模式` 下发送数据。

   命令：

   .. code-block:: none

     AT+CIPSEND

   响应：

   .. code-block:: none

     OK

     >

#. 停止发送数据

   在透传发送数据过程中，若识别到单独的一包数据 ``+++``，则系统会退出透传发送。此时请至少等待 1 秒，再发下一条 AT 命令。请注意，如果直接用键盘打字输入 ``+++``，有可能因时间太慢而不能被识别为连续的三个 ``+``。更多介绍请参考 :ref:`[仅适用透传模式] +++ <cmd-PLUS>`。

   .. Important::

     使用 ``+++`` 可退出透传发送数据，回到正常 AT 命令模式，此时 TCP 连接仍然有效。您也可以使用 ``AT+CIPSEND`` 命令恢复透传。

#. 退出 UART Wi-Fi 透传模式。

   命令：

   .. code-block:: none

     AT+CIPMODE=0

   响应：

   .. code-block:: none

     OK

#. 关闭 TCP 连接。

   命令：

   .. code-block:: none

     AT+CIPCLOSE

   响应：

   .. code-block:: none

     CLOSED

     OK

ESP 设备作为 SoftAP 在 UDP 传输中实现 UART Wi-Fi 透传
---------------------------------------------------------------------------------------------------------

#. 设置 Wi-Fi 模式为 SoftAP。

   命令：

   .. code-block:: none

     AT+CWMODE=2

   响应：

   .. code-block:: none

     OK

#. 设置 SoftAP。

   命令：

   .. code-block:: none

     AT+CWSAP="ESP32_SOFTAP","1234567890",5,3

   响应：

   .. code-block:: none

     OK

#. PC 连接到 ESP 设备的 SoftAP。

   .. figure:: ../../img/Connect-SoftAP.png
       :scale: 100 %
       :align: center
       :alt: Connect SoftAP

#. 创建一个 UDP 端点。

   在 PC 上使用网络调试助手，创建一个 UDP 传输。例如 PC 端 IP 地址为 ``192.168.4.2``，端口为 ``8080``。

#. ESP 设备与 PC 对应端口建立固定对端 IP 地址和端口的 UDP 传输。远程 IP 地址为 ``192.168.4.2``，远端端口为 ``8080``，本地端口为 ``2233``，模式为 ``0``。

   命令：

   .. code-block:: none

     AT+CIPSTART="UDP","192.168.4.2",8080,2233,0

   响应：

   .. code-block:: none

     CONNECT

     OK

#. 使能 UART Wi-Fi 透传模式。

   命令：

   .. code-block:: none

     AT+CIPMODE=1

   响应：

   .. code-block:: none

     OK

#. 在 :term:`透传模式` 下发送数据。

   命令：

   .. code-block:: none

     AT+CIPSEND

   响应：

   .. code-block:: none

     OK

     >

#. 停止发送数据

   在透传发送数据过程中，若识别到单独的一包数据 ``+++``，则系统会退出透传发送。此时请至少等待 1 秒，再发下一条 AT 命令。请注意，如果直接用键盘打字输入 ``+++``，有可能因时间太慢而不能被识别为连续的三个 ``+``。更多介绍请参考 :ref:`[仅适用透传模式] +++ <cmd-PLUS>`。

   .. Important::

     使用 ``+++`` 可退出透传发送数据，回到正常 AT 命令模式，此时 TCP 连接仍然有效。您也可以使用 ``AT+CIPSEND`` 命令恢复透传。

#. 退出 UART Wi-Fi 透传模式。

   命令：

   .. code-block:: none

     AT+CIPMODE=0

   响应：

   .. code-block:: none

     OK

#. 关闭 TCP 连接。

   命令：

   .. code-block:: none

     AT+CIPCLOSE

   响应：

   .. code-block:: none

     CLOSED

     OK
