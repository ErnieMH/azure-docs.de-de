---
title: Sicheres Speichern von Geheimnis-Anwendungseinstellungen für eine Webanwendung – Azure Key Vault | Microsoft-Dokumentation
description: Informationen zum sicheren Speichern von Geheimnis-Anwendungseinstellungen, wie etwa Azure-Anmeldeinformationen oder API-Schlüsseln von Drittanbietern, mithilfe des Key Vault-Anbieters von ASP.NET Core, User Secret oder der .NET 4.7.1-Konfigurationsbuilder
services: visualstudio
author: cawaMS
manager: paulyuk
editor: ''
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 07/17/2019
ms.author: cawa
ms.custom: devx-track-csharp
ms.openlocfilehash: 79fa01e53b53f3066e55736c105d6489ccbd96e7
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "89019843"
---
# <a name="securely-save-secret-application-settings-for-a-web-application"></a>Sicheres Speichern von Geheimnis-Anwendungseinstellungen für eine Webanwendung

## <a name="overview"></a>Übersicht
In diesem Artikel wird beschrieben, wie Geheimnis-Anwendungskonfigurationseinstellungen für Azure-Anwendungen sicher gespeichert werden können.

Üblicherweise werden alle Konfigurationseinstellungen für Webanwendungen in Konfigurationsdateien wie etwa „Web.config“ gespeichert. Diese Praxis führt dazu, dass Geheimniseinstellungen wie Cloudanmeldeinformationen bei öffentlichen Quellcodeverwaltungs-Systemen wie GitHub eingecheckt werden. Mittlerweile ist es aber aufgrund des erforderlichen Mehraufwands für Quellcodeänderungen und die Neukonfiguration von Entwicklungseinstellungen schwierig geworden, bewährte Sicherheitsmethoden zu befolgen.

Um zu gewährleisten, dass der Entwicklungsprozess sicher ist, werden Tool- und Frameworkbibliotheken erstellt, um Geheimnis-Anwendungseinstellungen mit minimalen oder gar keinen Änderungen am Quellcode sicher zu speichern.

## <a name="aspnet-and-net-core-applications"></a>ASP.NET- und .NET Core-Anwendungen

### <a name="save-secret-settings-in-user-secret-store-that-is-outside-of-source-control-folder"></a>Speichern Sie Geheimniseinstellungen im User Secret-Speicher, der sich außerhalb des Ordners für die Quellcodeverwaltung befindet.
Wenn Sie schnell einen Prototyp erstellen oder keinen Internetzugriff haben, verschieben Sie zunächst Ihre Geheimniseinstellungen aus dem Ordner für die Quellcodeverwaltung in den User Secret-Speicher. Der User Secret-Speicher ist eine Datei, die unter dem Benutzerprofilordner gespeichert wird, sodass Geheimnisse nicht in die Quellcodeverwaltung eingecheckt werden. Im folgenden Diagramm ist dargestellt, wie [User Secret](https://docs.microsoft.com/aspnet/core/security/app-secrets?tabs=visual-studio) funktioniert.

![User Secret bewahrt Geheimniseinstellungen außerhalb der Quellcodeverwaltung auf](../media/vs-secure-secret-appsettings/aspnetcore-usersecret.PNG)

Wenn Sie eine .NET Core-Konsolenanwendung ausführen, verwenden Sie Key Vault, um Ihr Geheimnis sicher zu speichern.

### <a name="save-secret-settings-in-azure-key-vault"></a>Speichern von Geheimniseinstellungen in Azure Key Vault
Wenn Sie ein Projekt entwickeln und Quellcode in sicherer Weise teilen müssen, verwenden Sie [Azure Key Vault](https://azure.microsoft.com/services/key-vault/).

1. Erstellen Sie einen Key Vault in Ihrem Azure-Abonnement. Füllen Sie auf der Benutzeroberfläche alle Pflichtfelder aus, und klicken Sie unten auf dem Blatt auf *Erstellen*

    ![Azure Key Vault erstellen](../media/vs-secure-secret-appsettings/create-keyvault.PNG)

2. Erteilen Sie sich und Ihren Teammitgliedern Zugriff auf den Key Vault. Bei einem großen Team können Sie eine [Azure Active Directory-Gruppe](../../active-directory/active-directory-groups-create-azure-portal.md) erstellen und dem Key Vault den Zugriff auf diese Sicherheitsgruppe hinzufügen. Markieren Sie in der Dropdownliste *Berechtigungen für Geheimnis* die Optionen *Abrufen* und *Auflisten* unter *Verwaltungsvorgänge für Geheimnisse*.
Wenn Sie Ihre Web-App bereits erstellt haben, gewähren Sie der Web-App Zugriff auf Key Vault, damit Sie auf den Schlüsseltresor zugreifen kann, ohne die Geheimniskonfiguration in den App-Einstellungen oder in Dateien zu speichern. Suchen Sie nach ihrer Web-App anhand ihres Namens, und fügen Sie sie auf die gleiche Weise hinzu, wie Sie Benutzern Zugriff gewähren.

    ![Hinzufügen der Key Vault-Zugriffsrichtlinie](../media/vs-secure-secret-appsettings/add-keyvault-access-policy.png)

3. Fügen Sie Key Vault im Azure-Portal Ihr Geheimnis hinzu. Ersetzen Sie in geschachtelten Konfigurationseinstellungen ‚:‘ durch ‚--‘, damit der Name des Key Vault-Geheimnisses gültig ist. ‚:‘ ist im Namen von Key Vault-Geheimnissen unzulässig.

    ![Hinzufügen eines Key Vault-Geheimnisses](../media/vs-secure-secret-appsettings/add-keyvault-secret.png)

    > [!NOTE]
    > Vor Visual Studio 2017 V15.6 wurde die Installation der Azure Services Authentication-Erweiterung für Visual Studio empfohlen. Aber diese ist jetzt veraltet, da die Funktionalität in Visual Studio integriert ist. Wenn Sie also eine ältere Version von Visual Studio 2017 verwenden, wird empfohlen, auf mindestens VS 2017 15.6 oder höher zu aktualisieren, damit Sie diese Funktionalität nativ nutzen und über die Visual Studio-Anmeldeidentität selbst auf den Schlüsseltresor zugreifen können.
    >

4. Fügen Sie Ihrem Projekt die folgenden NuGet-Pakete hinzu:

    ```
    Microsoft.Azure.KeyVault
    Microsoft.Azure.Services.AppAuthentication
    Microsoft.Extensions.Configuration.AzureKeyVault
    ```
5. Fügen Sie der Datei „Program.cs“ folgenden Code hinzu:

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
             Host.CreateDefaultBuilder(args)
                .ConfigureAppConfiguration((ctx, builder) =>
                {
                    var keyVaultEndpoint = GetKeyVaultEndpoint();
                    if (!string.IsNullOrEmpty(keyVaultEndpoint))
                    {
                        var azureServiceTokenProvider = new AzureServiceTokenProvider();
                        var keyVaultClient = new KeyVaultClient(
                            new KeyVaultClient.AuthenticationCallback(
                                azureServiceTokenProvider.KeyVaultTokenCallback));
                        builder.AddAzureKeyVault(
                        keyVaultEndpoint, keyVaultClient, new DefaultKeyVaultSecretManager());
                    }
                })
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });

        private static string GetKeyVaultEndpoint() => Environment.GetEnvironmentVariable("KEYVAULT_ENDPOINT");
    ```
6. Fügen Sie Ihre Key Vault-URL der Datei „launchsettings.json“ hinzu. Der Name der Umgebungsvariablen *KEYVAULT_ENDPOINT* ist in dem Code definiert, den Sie in Schritt 6 hinzugefügt haben.

    ![Hinzufügen der Key Vault-URL als Projektumgebungsvariable](../media/vs-secure-secret-appsettings/add-keyvault-url.png)

7. Beginnen Sie mit dem Debuggen des Projekts. Es sollte erfolgreich ausgeführt werden.

## <a name="aspnet-and-net-applications"></a>ASP.NET- und .NET-Anwendungen

.NET 4.7.1 unterstützt Key Vault und Geheimniskonfigurationsbuilder, wodurch sichergestellt ist, dass Geheimnisse ohne Codeänderungen aus dem Ordner für die Quellcodeverwaltung verschoben werden können.
Um fortzufahren, [laden Sie .NET 4.7.1 herunter](https://www.microsoft.com/download/details.aspx?id=56115), und migrieren Sie Ihre Anwendung, wenn sie eine ältere Version von .NET Framework verwendet.

### <a name="save-secret-settings-in-a-secret-file-that-is-outside-of-source-control-folder"></a>Speichern der Geheimniseinstellungen in einer Geheimnisdatei außerhalb des Ordners für die Quellcodeverwaltung
Wenn Sie einen schnellen Prototyp entwickeln und keine Azure-Ressourcen bereitstellen möchten, verwenden Sie diese Option.

1. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **Benutzergeheimnisse verwalten** aus. Dadurch wird das NuGet-Paket **Microsoft.Configuration.ConfigurationBuilders.UserSecrets** installiert, eine Datei zum Speichern geheimer Einstellungen außerhalb der Datei „web.config“ erstellt und der Abschnitt **ConfigBuilders** in der Datei „web.config“ hinzugefügt.

2. Legen Sie geheime Einstellungen unter dem Stammelement ab. Unten finden Sie ein Beispiel.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <root>
      <secrets ver="1.0">
        <secret name="secret" value="foo"/>
        <secret name="secret1" value="foo_one" />
        <secret name="secret2" value="foo_two" />
      </secrets>
    </root>
    ```

3. Geben Sie an, dass der Abschnitt „appSettings“ den Geheimniskonfigurationsbuilder verwendet. Achten Sie darauf, dass die Geheimniseinstellung einen Dummywert enthält.

    ```xml
        <appSettings configBuilders="Secrets">
            <add key="webpages:Version" value="3.0.0.0" />
            <add key="webpages:Enabled" value="false" />
            <add key="ClientValidationEnabled" value="true" />
            <add key="UnobtrusiveJavaScriptEnabled" value="true" />
            <add key="secret" value="" />
        </appSettings>
    ```

5. Debuggen Sie Ihre App. Es sollte erfolgreich ausgeführt werden.

### <a name="save-secret-settings-in-an-azure-key-vault"></a>Speichern von Geheimniseinstellungen in einem Azure Key Vault
Befolgen Sie die Anweisungen im ASP.NET Core-Abschnitt, um einen Key Vault für Ihr Projekt zu konfigurieren.

1. Installieren Sie das folgende NuGet-Paket in Ihrem Projekt.
   ```
   Microsoft.Configuration.ConfigurationBuilders.Azure
   ```

2. Definieren Sie den Key Vault-Konfigurationsbuilder in „Web.config“. Platzieren Sie diesen Abschnitt vor dem Abschnitt *appSettings*. Ersetzen Sie *vaultName* durch den Key Vault-Namen, wenn sich Ihr Key Vault im globalen Azure befindet, oder durch den vollständigen URI, wenn Sie eine unabhängige Cloud verwenden.

    ```xml
     <configBuilders>
        <builders>
            <add name="Secrets" userSecretsId="695823c3-6921-4458-b60b-2b82bbd39b8d" type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.UserSecrets, Version=2.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" />
            <add name="AzureKeyVault" vaultName="[VaultName]" type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Azure, Version=2.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" />
        </builders>
      </configBuilders>
    ```
3. Geben Sie an, dass der Abschnitt „appSettings“ den Key Vault-Konfigurationsbuilder verwendet. Achten Sie darauf, dass die Geheimniseinstellung einen beliebigen Dummywert enthält.

   ```xml
   <appSettings configBuilders="AzureKeyVault">
       <add key="webpages:Version" value="3.0.0.0" />
       <add key="webpages:Enabled" value="false" />
       <add key="ClientValidationEnabled" value="true" />
       <add key="UnobtrusiveJavaScriptEnabled" value="true" />
       <add key="secret" value="" />
   </appSettings>
   ```

4. Beginnen Sie mit dem Debuggen des Projekts. Es sollte erfolgreich ausgeführt werden.
