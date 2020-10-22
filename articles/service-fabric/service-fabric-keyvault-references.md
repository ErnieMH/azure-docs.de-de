---
title: 'Azure Service Fabric: Verwenden der KeyVault-Verweise von Service Fabric-Anwendungen'
description: In diesem Artikel wird erläutert, wie Sie die KeyVaultReference-Unterstützung von Service Fabric für Anwendungsgeheimnisse verwenden.
ms.topic: article
ms.date: 09/20/2019
ms.openlocfilehash: f2221bb3e8e3ee3181b2cff70107dccc203954cf
ms.sourcegitcommit: ce8eecb3e966c08ae368fafb69eaeb00e76da57e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2020
ms.locfileid: "92313797"
---
# <a name="keyvaultreference-support-for-service-fabric-applications-preview"></a>KeyVaultReference-Unterstützung für Service Fabric-Anwendungen (Vorschauversion)

Eine häufige Herausforderung bei der Erstellung von Cloudanwendungen ist die sichere Speicherung der von Ihrer Anwendung benötigten Geheimnisse. Sie können die Anmeldeinformationen des Containerrepositorys z. B. im Schlüsseltresor speichern und auf sie im Anwendungsmanifest verweisen. „KeyVaultReference“ von Service Fabric verwendet die verwaltete Service Fabric-Identität und gestaltet es einfach, auf Geheimnisse von Schlüsseltresoren zu verweisen. Im weiteren Verlauf dieses Artikels wird beschrieben, wie Sie „KeyVaultReference“ von Service Fabric verwenden können, und es werden einige typische Anwendungen erläutert.

> [!IMPORTANT]
> Die Verwendung dieses Vorschaufeatures wird in Produktionsumgebungen nicht empfohlen.

> [!NOTE]
> Die Vorschaufunktion KeyVaultReference unterstützt nur Geheimnisse [mit Versionsangabe](../key-vault/general/about-keys-secrets-certificates.md#objects-identifiers-and-versioning). Versionslose Geheimnisse werden nicht unterstützt.

## <a name="prerequisites"></a>Voraussetzungen

- Verwaltete Identität für Anwendung (MIT)
    
    Die KeyVaultReference-Unterstützung von Service Fabric verwendet die verwaltete Identität der Anwendung, weshalb Anwendungen, die die Verwendung von „KeyVaultReferences“ planen, die verwaltete Identität verwenden sollten. Befolgen Sie die Anweisungen in diesem [Dokument](concepts-managed-identity.md), um die verwaltete Identität für Ihre Anwendung zu aktivieren.

- Central Secrets Store (CSS)

    Central Secrets Store (CSS) ist der verschlüsselte lokale Cache für Geheimnisse von Service Fabric. CSS ist ein lokaler Geheimnisspeichercache, der verwendet wird, um vertrauliche Daten wie Kennwörter, Token und Schlüssel im Arbeitsspeicher verschlüsselt zu speichern. KeyVaultReference wird nach dem Abrufen in CSS zwischengespeichert.

    Fügen Sie das Folgende zu Ihrer Clusterkonfiguration unter `fabricSettings` hinzu, um alle erforderlichen Features für die KeyVaultReference-Unterstützung zu aktivieren.

    ```json
    "fabricSettings": 
    [
        ...
    {
                "name":  "CentralSecretService",
                "parameters":  [
                {
                    "name":  "IsEnabled",
                    "value":  "true"
                },
                {
                    "name":  "MinReplicaSetSize",
                    "value":  "3"
                },
                {
                    "name":  "TargetReplicaSetSize",
                    "value":  "3"
                }
                ]
            },
            {
                "name":  "ManagedIdentityTokenService",
                "parameters":  [
                {
                    "name":  "IsEnabled",
                    "value":  "true"
                }
                ]
            }
            ]
    ```

    > [!NOTE] 
    > Es wird empfohlen, ein separates Verschlüsselungszertifikat für CSS zu verwenden. Sie können es unter dem Abschnitt „CentralSecretService“ hinzufügen.
    

    ```json
        {
            "name": "EncryptionCertificateThumbprint",
            "value": "<EncryptionCertificateThumbprint for CSS>"
        }
    ```
Damit die Änderungen wirksam werden, müssen Sie auch die Upgraderichtlinie ändern, um auf jedem Knoten einen Neustart der Service Fabric-Runtime zu erzwingen, wenn das Upgrade den Cluster durchläuft. Mit diesem Neustart wird sichergestellt, dass der neu aktivierte Systemdienst auf jedem Knoten gestartet und ausgeführt wird. Im folgenden Codeausschnitt ist forceRestart die wesentliche Einstellung. Verwenden Sie Ihre vorhandenen Werte für die übrigen Einstellungen.
```json
"upgradeDescription": {
    "forceRestart": true,
    "healthCheckRetryTimeout": "00:45:00",
    "healthCheckStableDuration": "00:05:00",
    "healthCheckWaitDuration": "00:05:00",
    "upgradeDomainTimeout": "02:00:00",
    "upgradeReplicaSetCheckTimeout": "1.00:00:00",
    "upgradeTimeout": "12:00:00"
}
```
- Erteilen der Zugriffsberechtigung für verwaltete Identitäten der Anwendung auf den Schlüsseltresor

    Lesen Sie dieses [Dokument](how-to-grant-access-other-resources.md), um zu sehen, wie Sie dem Schlüsseltresor Zugriff auf verwaltete Identitäten erteilen können. Beachten Sie auch, dass bei Verwendung der systemseitig zugewiesenen verwalteten Identität die verwaltete Identität erst nach der Bereitstellung der Anwendung erstellt wird.

## <a name="keyvault-secret-as-application-parameter"></a>Geheimnis des Schlüsseltresors als Anwendungsparameter
Angenommen, die Anwendung muss das im Schlüsseltresor gespeicherte Kennwort der Back-End-Datenbank lesen, dann gestaltet die KeyVaultReference-Unterstützung von Service Fabric diesen Vorgang einfach. Das folgende Beispiel liest das Geheimnis `DBPassword` mithilfe der KeyVaultReference-Unterstützung von Service Fabric aus dem Schlüsseltresor.

- Hinzufügen eines Abschnitts zu „settings.xml“

    Definieren Sie den Parameter `DBPassword` mit dem Typ `KeyVaultReference` und dem Wert `<KeyVaultURL>`.

    ```xml
    <Section Name="dbsecrets">
        <Parameter Name="DBPassword" Type="KeyVaultReference" Value="https://vault200.vault.azure.net/secrets/dbpassword/8ec042bbe0ea4356b9b171588a8a1f32"/>
    </Section>
    ```
- Verweisen Sie auf den neuen Abschnitt in „ApplicationManifest.xml“ in `<ConfigPackagePolicies>`.

    ```xml
    <ServiceManifestImport>
        <Policies>
        <IdentityBindingPolicy ServiceIdentityRef="WebAdmin" ApplicationIdentityRef="ttkappuser" />
        <ConfigPackagePolicies CodePackageRef="Code">
            <!--Linux container example-->
            <ConfigPackage Name="Config" SectionName="dbsecrets" EnvironmentVariableName="SecretPath" MountPoint="/var/secrets"/>
            <!--Windows container example-->
            <!-- <ConfigPackage Name="Config" SectionName="dbsecrets" EnvironmentVariableName="SecretPath" MountPoint="C:\secrets"/> -->
        </ConfigPackagePolicies>
        </Policies>
    </ServiceManifestImport>
    ```

- Verwenden von KeyVaultReference in Ihrer Anwendung

    Die Dienstinstanziierung von Service Fabric löst den KeyVaultReference-Parameter unter Verwendung der verwalteten Identität der Anwendung auf. Jeder unter `<Section  Name=dbsecrets>` aufgeführte Parameter ist eine Datei unter dem Ordner, auf den „EnvironmentVariable SecretPath“ verweist. Der nachfolgende C#-Codeausschnitt zeigt, wie Sie „DBPassword“ in Ihrer Anwendung lesen können.

    ```C#
    string secretPath = Environment.GetEnvironmentVariable("SecretPath");
    using (StreamReader sr = new StreamReader(Path.Combine(secretPath, "DBPassword"))) 
    {
        string dbPassword =  sr.ReadToEnd();
        // dbPassword to connect to DB
    }
    ```
    > [!NOTE] 
    > Für das Containerszenario können Sie „MountPoint“ verwenden, um zu steuern, wo die `secrets` eingebunden werden sollen.

## <a name="keyvault-secret-as-environment-variable"></a>Geheimnis des Schlüsseltresors als Umgebungsvariable

Die Service Fabric-Umgebungsvariablen unterstützen jetzt den Typ „KeyVaultReference“. Das folgende Beispiel zeigt, wie eine Umgebungsvariable an ein in KeyVault gespeichertes Geheimnis gebunden werden kann.

```xml
<EnvironmentVariables>
      <EnvironmentVariable Name="EventStorePassword" Type="KeyVaultReference" Value="https://ttkvault.vault.azure.net/secrets/clustercert/e225bd97e203430d809740b47736b9b8"/>
</EnvironmentVariables>
```

```C#
string eventStorePassword =  Environment.GetEnvironmentVariable("EventStorePassword");
```
## <a name="keyvault-secret-as-container-repository-password"></a>Geheimnis des Schlüsseltresors als Kennwort für das Containerrepository
KeyVaultReference ist ein unterstützter Typ für Container-RepositoryCredentials. Das folgende Beispiel zeigt, wie Sie einen Schlüsseltresorverweis als Kennwort für ein Containerrepository verwenden können.
```xml
 <Policies>
      <ContainerHostPolicies CodePackageRef="Code">
        <RepositoryCredentials AccountName="user1" Type="KeyVaultReference" Password="https://ttkvault.vault.azure.net/secrets/containerpwd/e225bd97e203430d809740b47736b9b8"/>
      </ContainerHostPolicies>
```
## <a name="faq"></a>Häufig gestellte Fragen
- Die verwaltete Identität muss für die KeyVaultReference-Unterstützung aktiviert werden. Bei der Aktivierung Ihrer Anwendung tritt ein Fehler auf, wenn KeyVaultReference ohne die Aktivierung der verwalteten Identität verwendet wird.

- Wenn Sie eine vom System zugewiesene Identität verwenden, wird sie erst nach der Bereitstellung der Anwendung erstellt, wodurch eine Ringabhängigkeit entsteht. Sobald Ihre Anwendung bereitgestellt ist, können Sie dem System, dem die Identität zugewiesen wurde, die Zugriffsberechtigung für den Schlüsseltresor erteilen. Sie können die dem System zugewiesene Identität anhand des Namens {Cluster}/{Anwendungsname}/{Dienstname} finden.

- Der Schlüsseltresor muss sich in demselben Abonnement befinden wie Ihr Service Fabric-Cluster. 

## <a name="next-steps"></a>Nächste Schritte

* [Azure KeyVault-Dokumentation](../key-vault/index.yml)