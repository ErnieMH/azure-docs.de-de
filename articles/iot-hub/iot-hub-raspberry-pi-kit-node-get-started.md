---
title: Verbinden von Raspberry Pi mit Azure IoT Hub in der Cloud (Node.js)
description: Erfahren Sie in diesem Tutorial, wie Sie Raspberry Pi einrichten und eine Verbindung mit Azure IoT Hub herstellen, damit Raspberry Pi Daten an die Azure-Cloudplattform senden kann.
author: wesmc7777
manager: eliotgra
keywords: Azure IoT Raspberry Pi, Raspberry Pi IoT Hub, Raspberry Pi sendet Daten an Cloud, Raspberry Pi in der Cloud
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: wesmc
ms.custom:
- 'Role: Cloud Development'
- devx-track-js
ms.openlocfilehash: 1d6a51e2e9c052be0c59435b287c5fdde459f55d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91334190"
---
# <a name="connect-raspberry-pi-to-azure-iot-hub-nodejs"></a>Verbinden von Raspberry Pi mit Azure IoT Hub (Node.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

In diesem Tutorial erlernen Sie die Grundlagen der Verwendung von Raspberry Pi 3 und Raspbian. Anschließend erfahren Sie, wie Sie Ihre Geräte mithilfe von [Azure IoT Hub](about-iot-hub.md) nahtlos mit der Cloud verbinden. Beispiele für Windows 10 IoT Core finden Sie im [Windows Dev Center](https://www.windowsondevices.com/).

Sie haben noch kein Kit? Probieren Sie den [Raspberry Pi-Onlinesimulator](iot-hub-raspberry-pi-web-simulator-get-started.md) aus. Oder erwerben Sie [hier](https://azure.microsoft.com/develop/iot/starter-kits) ein neues Kit.

## <a name="what-you-do"></a>Aufgaben

* Erstellen Sie einen IoT Hub.

* Registrieren Sie ein Gerät für Pi in Ihrem IoT Hub.

* Richten Sie Raspberry Pi ein.

* Führen Sie eine Beispielanwendung auf Pi aus, um Sensordaten an Ihren IoT Hub zu senden.

## <a name="what-you-learn"></a>Lerninhalt

* Erstellen eines Azure IoT Hubs und Abrufen der Verbindungszeichenfolge für Ihr neues Gerät

* Herstellen der Verbindung von Pi mit einem BME280-Sensor

* Erfassen von Sensordaten durch Ausführen einer Beispielanwendung auf Pi

* Senden von Sensordaten an Ihren IoT Hub

## <a name="what-you-need"></a>Voraussetzungen

![Voraussetzungen](./media/iot-hub-raspberry-pi-kit-node-get-started/0-starter-kit.png)

* Eine Raspberry Pi 2- oder Raspberry Pi 3-Platine.

* Ein Azure-Abonnement. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

* Ein Monitor, eine USB-Tastatur und eine Maus, die mit Pi verbunden werden.

* Ein Mac oder PC, auf dem Windows oder Linux ausgeführt wird.

* Eine Internetverbindung.

* microSD-Karte mit mindestens 16 GB

* USB-SD-Adapter oder microSD-Karte, um das Betriebssystemimage auf die microSD-Karte zu kopieren

* Netzteil (5 V, 2 A) mit Micro-USB-Kabel (1,8 m)

Die folgenden Elemente sind optional:

* Ein zusammengebauter Adafruit BME280-Sensor für Temperatur, Luftdruck und Luftfeuchtigkeit

* Eine Steckplatine

* 6 F/M-Jumperdrähte

* 1 LED, diffus, 10 mm

> [!NOTE]
> Wenn Sie die optionalen Elemente nicht besitzen, können Sie simulierte Sensordaten verwenden.

## <a name="create-an-iot-hub"></a>Erstellen eines IoT-Hubs

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Registrieren eines neuen Geräts beim IoT-Hub

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="set-up-raspberry-pi"></a>Einrichten von Raspberry Pi

### <a name="install-the-raspbian-operating-system-for-pi"></a>Installieren des Betriebssystems Raspbian für Pi

Bereiten Sie die microSD-Karte für die Installation des Raspbian-Image vor.

1. Laden Sie Raspbian herunter.

   a. [Raspbian Buster mit Desktop](https://www.raspberrypi.org/downloads/raspbian/) (ZIP-Datei).

   b. Entpacken Sie das Raspbian-Image in einen Ordner auf Ihrem Computer.

2. Installieren Sie Raspbian auf der microSD-Karte.

   a. [Laden Sie das SD-Kartenbrennprogramm Etcher herunter, und installieren Sie es](https://etcher.io/).

   b. Führen Sie Etcher aus, und wählen Sie das Raspbian-Image, das Sie in Schritt 1 entpackt haben.

   c. Wählen Sie das microSD-Kartenlaufwerk. Etcher hat möglicherweise bereits das richtige Laufwerk ausgewählt.

   d. Klicken Sie auf „Flash“, um Raspbian auf der microSD-Karte zu installieren.

   e. Entfernen Sie die microSD-Karte aus dem Computer, wenn die Installation abgeschlossen ist. Es ist sicher, die microSD-Karte direkt zu entfernen, da Etcher die microSD-Karte nach Abschluss des Vorgangs automatisch auswirft bzw. die Bereitstellung aufhebt.

   f. Legen Sie die microSD-Karte in den Raspberry Pi ein.

### <a name="enable-ssh-and-i2c"></a>Aktivieren von SSH und I2C

1. Stellen Sie die Verbindung von Pi mit Monitor, Tastatur und Maus her.

2. Starten Sie Pi, und melden Sie sich bei Raspbian mit `pi` als Benutzername und `raspberry` als Kennwort an.

3. Klicken Sie auf das Raspberry-Symbol und dann auf **Preferences** > **Raspberry Pi Configuration**.

   ![Das Raspbian-Menü „Preferences“](./media/iot-hub-raspberry-pi-kit-node-get-started/1-raspbian-preferences-menu.png)

4. Legen Sie auf der Registerkarte **Interfaces** die Einstellungen **I2C** und **SSH** auf **Enable** fest, und klicken Sie dann auf **OK**. Wenn Sie keine physischen Sensoren besitzen und simulierte Sensordaten verwenden möchten, ist dieser Schritt optional.

   ![Aktivieren von I2C und SSH auf Raspberry Pi](./media/iot-hub-raspberry-pi-kit-node-get-started/2-enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE]
> Weitere Referenzdokumente zum Aktivieren von SSH und I2C finden Sie auf[raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) und unter [RASPI-CONFIG](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).

### <a name="connect-the-sensor-to-pi"></a>Verbinden des Sensors mit Pi

Verwenden Sie die Steckplatine und Jumperdrähte, um eine LED und einen BME280-Sensor wie folgt zu verbinden. Wenn Sie keinen Sensor haben, [überspringen Sie diesen Abschnitt](#connect-pi-to-the-network).

![Die Raspberry Pi- und Sensorverbindung](./media/iot-hub-raspberry-pi-kit-node-get-started/3-raspberry-pi-sensor-connection.png)

Mit dem BME280-Sensor können Daten zur Temperatur und Luftfeuchtigkeit erfasst werden. Die LED blinkt, wenn das Gerät eine Nachricht an die Cloud sendet.

Verwenden Sie für Sensorstifte die folgende Verkabelung:

| Start (Sensor und LED)     | Ende (Board)            | Kabelfarbe   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Stift 5G)             | 3,3 V PWR (Stift 1)       | Weißes Kabel   |
| GND (Stift 7G)             | GND (Stift 6)            | Braunes Kabel   |
| SDI (Stift 10G)            | I2C1 SDA (Stift 3)       | Rotes Kabel     |
| SCK (Stift 8G)             | I2C1 SCL (Stift 5)       | Oranges Kabel  |
| LED VDD (Stift 18F)        | GPIO 24 (Stift 18)       | Weißes Kabel   |
| LED GND (Stift 17F)        | GND (Stift 20)           | Schwarzes Kabel   |

Klicken Sie hier, um die [Raspberry Pi 2- und 3-Stiftzuordnungen](/windows/iot-core/learn-about-hardware/pinmappings/pinmappingsrpi) zur Referenz anzuzeigen.

Nachdem Sie den BME280 erfolgreich mit Ihrem Raspberry Pi verbunden haben, sollte das Gerät wie in der nachstehenden Abbildung aussehen.

![Verbindung zwischen Pi und BME280](./media/iot-hub-raspberry-pi-kit-node-get-started/4-connected-pi.png)

### <a name="connect-pi-to-the-network"></a>Verbindung zwischen Pi und dem Netzwerk

Verbinden Sie den Raspberry Pi mit dem Micro-USB-Kabel mit der Stromversorgung. Verwenden Sie das Ethernet-Kabel zum Verbinden von Pi mit Ihrem verkabelten Netzwerk, oder befolgen Sie die [Anweisungen der Raspberry Pi Foundation](https://www.raspberrypi.org/documentation/configuration/wireless/), um Pi mit Ihrem WLAN zu verbinden. Notieren Sie sich die [IP-Adresse von Pi](https://www.raspberrypi.org/documentation/remote-access/ip-address.md), nachdem es eine Verbindung zum Netzwerk hergestellt hat.

![Mit verkabeltem Netzwerk verbunden](./media/iot-hub-raspberry-pi-kit-node-get-started/5-power-on-pi.png)

> [!NOTE]
> Stellen Sie sicher, dass der Raspberry Pi mit dem gleichen Netzwerk wie der Computer verbunden ist. Wenn der Computer beispielsweise mit einem Drahtlosnetzwerk und Pi mit einem verkabelten Netzwerk verbunden ist, wird in der Ausgabe des devdisco-Befehls die IP-Adresse möglicherweise nicht angezeigt.

## <a name="run-a-sample-application-on-pi"></a>Ausführen einer Beispielanwendung auf Pi

### <a name="clone-sample-application-and-install-the-prerequisite-packages"></a>Klonen der Beispielanwendung und Installieren der Pakete mit den erforderlichen Komponenten

1. Stellen Sie die Verbindung Ihres Raspberry Pi mit einem der folgenden SSH-Clients auf Ihrem Hostcomputer her:

   **Windows-Benutzer**

   a. Laden Sie [PuTTY](https://www.putty.org/) für Windows herunter, und installieren Sie es.

   b. Kopieren Sie die IP-Adresse von Pi, fügen Sie sie in den Abschnitt „Hostname (oder IP-Adresse)“ ein, und wählen Sie „SSH“ als Verbindungstyp.

   ![PuTTY](./media/iot-hub-raspberry-pi-kit-node-get-started/7-putty-windows.png)

   **Mac- und Ubuntu-Benutzer**

   Verwenden Sie den integrierten SSH-Client unter Ubuntu oder macOS. Möglicherweise müssen Sie `ssh pi@<ip address of pi>` ausführen, um eine SSH-Verbindung mit Pi herzustellen.

   > [!NOTE]
   > Der Standardbenutzername ist `pi`, und das Kennwort ist `raspberry`.

2. Installieren von Node.js und NPM auf Pi

   Überprüfen Sie zunächst Ihre Node.js-Version.

   ```bash
   node -v
   ```

   Wenn die Version niedriger als 10.x oder kein Node.js auf Ihrem Pi vorhanden ist, installieren Sie die neueste Version.

   ```bash
   curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

3. Klonen Sie die Beispielanwendung.

   ```bash
   git clone https://github.com/Azure-Samples/azure-iot-samples-node.git
   ```

4. Installieren Sie alle Pakete für das Beispiel. Die Installation umfasst das Azure IoT-Geräte-SDK, die BME280-Sensorbibliothek und die Wiring-Pi-Bibliothek zur Verkabelung.

   ```bash
   cd azure-iot-samples-node/iot-hub/Tutorials/RaspberryPiApp
   npm install
   ```

   > [!NOTE]
   >Je nach Netzwerkverbindung kann das Abschließen dieses Installationsvorgangs mehrere Minuten dauern.

### <a name="configure-the-sample-application"></a>Konfigurieren der Beispielanwendung

1. Öffnen Sie die Konfigurationsdatei durch Ausführen der folgenden Befehle:

   ```bash
   nano config.json
   ```

   ![Konfigurationsdatei](./media/iot-hub-raspberry-pi-kit-node-get-started/6-config-file.png)

   Es gibt zwei Argumente in dieser Datei, die Sie konfigurieren können. Das erste ist `interval`, mit dem das Zeitintervall (in Millisekunden) zwischen Nachrichten bestimmt wird, die an die Cloud gesendet werden. Das zweite heißt `simulatedData` und ist ein boolescher Wert, der bestimmt, ob simulierte Sensordaten verwendet werden sollen.

   Wenn Sie **keinen Sensor haben**, legen Sie den Wert von `simulatedData` auf `true` fest, damit die Beispielanwendung simulierte Sensordaten erstellt und nutzt.

   *Hinweis: Die in diesem Tutorial verwendete i2c-Adresse ist standardmäßig „0x77“. Je nach Ihrer Konfiguration könnte sie auch „0x76“ lauten. Wenn ein i2c-Fehler auftritt, versuchen Sie, den Wert in „118“ zu ändern, und prüfen Sie, ob dies besser funktioniert. Wenn Sie sehen möchten, welche Adresse von Ihrem Sensor verwendet wird, führen Sie `sudo i2cdetect -y 1` in einer Shell auf Raspberry Pi aus*.

2. Speichern und beenden Sie durch Drücken von STRG+O > EINGABETASTE > STRG+X.

### <a name="run-the-sample-application"></a>Ausführen der Beispielanwendung

Führen Sie die Beispielanwendung durch Aufrufen des folgenden Befehls aus:

   ```bash
   sudo node index.js '<YOUR AZURE IOT HUB DEVICE CONNECTION STRING>'
   ```

   > [!NOTE]
   > Stellen Sie sicher, dass Sie die Verbindungszeichenfolge des Geräts kopieren und zwischen einfachen Anführungszeichen einfügen.

Die folgende Ausgabe sollte angezeigt werden, die die Sensordaten und Nachrichten zeigt, die an Ihren IoT Hub gesendet werden.

![Ausgabe: Von Raspberry Pi an Ihren IoT Hub gesendete Sensordaten](./media/iot-hub-raspberry-pi-kit-node-get-started/8-run-output.png)

## <a name="read-the-messages-received-by-your-hub"></a>Lesen der von Ihrem Hub empfangenen Nachrichten

Wenn Sie die von Ihrem IoT-Hub empfangenen Nachrichten von Ihrem Gerät aus überwachen möchten, können Sie dafür die Azure IoT-Tools für Visual Studio Code verwenden. Weitere Informationen finden Sie unter [Senden und Empfangen von Nachrichten zwischen Ihrem Gerät und IoT Hub mithilfe der Azure IoT-Tools für Visual Studio Code](iot-hub-vscode-iot-toolkit-cloud-device-messaging.md).

Weitere Informationen zum Verarbeiten von Daten, die von Ihrem Gerät gesendet wurden, finden Sie im nächsten Abschnitt.

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine Beispielanwendung ausgeführt, die Sensordaten sammelt und an Ihren IoT Hub sendet.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
