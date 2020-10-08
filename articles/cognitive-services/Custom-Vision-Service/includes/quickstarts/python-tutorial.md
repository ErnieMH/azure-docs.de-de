---
author: areddish
ms.author: areddish
ms.service: cognitive-services
ms.date: 09/15/2020
ms.openlocfilehash: a091222b01669c6b83c599787c61dcd6b62b05d0
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/05/2020
ms.locfileid: "90604969"
---
Dieser Leitfaden enthält Anweisungen und Beispielcode für die ersten Schritte mit der Custom Vision-Clientbibliothek für Python und unterstützt Sie beim Erstellen eines Bildklassifizierungsmodells. Sie erstellen ein Projekt, fügen Tags hinzu, trainieren das Projekt und verwenden die Vorhersageendpunkt-URL des Projekts, um es programmgesteuert zu testen. Verwenden Sie dieses Beispiel als Vorlage für die Erstellung Ihrer eigenen Bilderkennungsanwendung.

> [!NOTE]
> Falls Sie ein Klassifizierungsmodell erstellen und trainieren möchten, _ohne_ Code zu schreiben, sehen Sie sich stattdessen die [browserbasierte Anleitung](../../getting-started-build-a-classifier.md) an.

## <a name="prerequisites"></a>Voraussetzungen

- [Python 2.7+ oder 3.5+](https://www.python.org/downloads/)
- [Pip](https://pip.pypa.io/en/stable/installing/)-Tool
- [!INCLUDE [create-resources](../../includes/create-resources.md)]

## <a name="install-the-custom-vision-client-library"></a>Installieren der Custom Vision-Clientbibliothek

Zum Schreiben einer Bildanalyseanwendung mit Custom Vision für Python benötigen Sie die Clientbibliothek für Custom Vision. Führen Sie in PowerShell den folgenden Befehl aus:

```powershell
pip install azure-cognitiveservices-vision-customvision
```

[!INCLUDE [get-keys](../../includes/get-keys.md)]

[!INCLUDE [python-get-images](../../includes/python-get-images.md)]

## <a name="add-the-code"></a>Hinzufügen des Codes

Erstellen Sie in Ihrem bevorzugten Projektverzeichnis eine neue Datei namens *sample.py*.

## <a name="create-the-custom-vision-project"></a>Erstellen des Custom Vision-Projekts

Fügen Sie Ihrem Skript den folgenden Code hinzu, um ein neues Custom Vision Service-Projekt zu erstellen. Fügen Sie Ihre Abonnementschlüssel in die entsprechenden Definitionen ein. Ermitteln Sie außerdem Ihre Endpunkt-URL über die Seite „Einstellungen“ der Custom Vision-Website.

Informationen zur Angabe weiterer Optionen bei der Erstellung Ihres Projekts finden Sie im Artikel zur Methode [create_project](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-customvision/azure.cognitiveservices.vision.customvision.training.operations.customvisiontrainingclientoperationsmixin?view=azure-python#create-project-name--description-none--domain-id-none--classification-type-none--target-export-platforms-none--custom-headers-none--raw-false----operation-config-). Informationen zur Projekterstellung finden Sie unter [Schnellstart: Erstellen einer Klassifizierung mit Custom Vision](../../getting-started-build-a-classifier.md).  

```Python
from azure.cognitiveservices.vision.customvision.training import CustomVisionTrainingClient
from azure.cognitiveservices.vision.customvision.training.models import ImageFileCreateBatch, ImageFileCreateEntry
from msrest.authentication import ApiKeyCredentials

ENDPOINT = "<your API endpoint>"

# Replace with a valid key
training_key = "<your training key>"
prediction_key = "<your prediction key>"
prediction_resource_id = "<your prediction resource id>"

publish_iteration_name = "classifyModel"

credentials = ApiKeyCredentials(in_headers={"Training-key": training_key})
trainer = CustomVisionTrainingClient(ENDPOINT, credentials)

# Create a new project
print ("Creating project...")
project = trainer.create_project("My New Project")
```

## <a name="create-tags-in-the-project"></a>Erstellen von Tags im Projekt

Um Ihrem Projekt Klassifizierungstags hinzuzufügen, fügen Sie den folgenden Code am Ende der Datei *sample.py* hinzu:

```Python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

## <a name="upload-and-tag-images"></a>Hochladen und Kennzeichnen von Bildern

Um dem Projekt die Beispielbilder hinzuzufügen, fügen Sie nach der Erstellung der Kategorien den folgenden Code ein. Dieser Code lädt das Bild mit dem entsprechenden Tag hoch. Sie können bis zu 64 Bilder in einem Batch hochladen.

> [!NOTE]
> Sie müssen den Pfad zu den Bildern gemäß dem Ort ändern, an den Sie das Repository mit den Cognitive Services-Beispielen für das Python SDK heruntergeladen haben.

```Python
base_image_url = "<path to repo directory>/cognitive-services-python-sdk-samples/samples/vision/"

print("Adding images...")

image_list = []

for image_num in range(1, 11):
    file_name = "hemlock_{}.jpg".format(image_num)
    with open(base_image_url + "images/Hemlock/" + file_name, "rb") as image_contents:
        image_list.append(ImageFileCreateEntry(name=file_name, contents=image_contents.read(), tag_ids=[hemlock_tag.id]))

for image_num in range(1, 11):
    file_name = "japanese_cherry_{}.jpg".format(image_num)
    with open(base_image_url + "images/Japanese Cherry/" + file_name, "rb") as image_contents:
        image_list.append(ImageFileCreateEntry(name=file_name, contents=image_contents.read(), tag_ids=[cherry_tag.id]))

upload_result = trainer.create_images_from_files(project.id, ImageFileCreateBatch(images=image_list))
if not upload_result.is_batch_successful:
    print("Image batch upload failed.")
    for image in upload_result.images:
        print("Image status: ", image.status)
    exit(-1)
```

## <a name="train-and-publish-the-project"></a>Trainieren und Veröffentlichen des Projekts

Dieser Code erstellt die erste Iteration des Vorhersagemodells und veröffentlicht diese anschließend am Vorhersageendpunkt. Der Name der veröffentlichten Iteration kann zum Senden von Vorhersageanforderungen verwendet werden. Eine Iteration ist erst im Vorhersageendpunkt verfügbar, wenn sie veröffentlicht wurde.

```Python
import time

print ("Training...")
iteration = trainer.train_project(project.id)
while (iteration.status != "Completed"):
    iteration = trainer.get_iteration(project.id, iteration.id)
    print ("Training status: " + iteration.status)
    time.sleep(1)

# The iteration is now trained. Publish it to the project endpoint
trainer.publish_iteration(project.id, iteration.id, publish_iteration_name, prediction_resource_id)
print ("Done!")
```

## <a name="use-the-prediction-endpoint"></a>Verwenden des Vorhersageendpunkts

Um ein Bild an den Vorhersageendpunkt zu senden und die Vorhersage abzurufen, fügen Sie am Ende der Datei den folgenden Code hinzu:

```python
from azure.cognitiveservices.vision.customvision.prediction import CustomVisionPredictionClient
from msrest.authentication import ApiKeyCredentials

# Now there is a trained endpoint that can be used to make a prediction
prediction_credentials = ApiKeyCredentials(in_headers={"Prediction-key": prediction_key})
predictor = CustomVisionPredictionClient(ENDPOINT, prediction_credentials)

with open(base_image_url + "images/Test/test_image.jpg", "rb") as image_contents:
    results = predictor.classify_image(
        project.id, publish_iteration_name, image_contents.read())

    # Display the results.
    for prediction in results.predictions:
        print("\t" + prediction.tag_name +
              ": {0:.2f}%".format(prediction.probability * 100))
```

## <a name="run-the-application"></a>Ausführen der Anwendung

Führen Sie *sample.py* aus.

```powershell
python sample.py
```

Die Ausgabe der Anwendung sollte dem folgenden Text ähneln:

```console
Creating project...
Adding images...
Training...
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```

Daraufhin können Sie sich vergewissern, dass das Testbild (unter **<Basis-URL der Bilder>/Images/Test/** ) ordnungsgemäß gekennzeichnet ist. Sie können auch zur [Custom Vision-Website](https://customvision.ai) zurückgehen und den aktuellen Status Ihres neu erstellten Projekts ansehen.

[!INCLUDE [clean-ic-project](../../includes/clean-ic-project.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie wissen nun, wie die einzelnen Schritte des Objekterkennungsprozesses im Code ausgeführt werden. In diesem Beispiel wird eine einzelne Trainingsiteration ausgeführt. Zur Verbesserung der Genauigkeit muss ein Modell jedoch häufig mehrmals trainiert und getestet werden.

> [!div class="nextstepaction"]
> [Testen und erneutes Trainieren eines Modells mit Custom Vision Service](../../test-your-model.md)

* [Was ist Custom Vision?](../../overview.md)
* [SDK-Referenzdokumentation](https://docs.microsoft.com/python/api/overview/azure/cognitiveservices/customvision?view=azure-python)