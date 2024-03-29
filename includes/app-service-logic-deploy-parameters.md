Gestione risorse di Azure permette di definire i parametri per i valori da specificare durante la distribuzione del modello. Il modello include una sezione denominata Parametri che contiene tutti i valori dei parametri. È necessario definire un parametro per i valori che variano in base al progetto distribuito o all'ambiente in cui viene distribuito il progetto. Non definire i parametri per i valori che rimangono invariati. Ogni valore di parametro nel modello viene usato per definire le risorse distribuite.

Durante la definizione dei parametri, usare il campo **allowedValues** per specificare i valori che l'utente può fornire durante la distribuzione. Usare il campo **defaultValue** per assegnare un valore al parametro, se non viene specificato alcun valore durante la distribuzione.

Di seguito è riportata la descrizione di ogni parametro del modello.

### logicAppName

Nome dell'app per la logica da creare.

    "logicAppName": {
        "type": "string"
    }

### hostingPlanName

Nome del piano di servizio app da creare per l'hosting dell'app per la logica.
    
    "hostingPlanName": {
      "type": "string",
      "metadata": {
        "description": "The name of the App Service plan to create for hosting the logic app."
      }
    }

### flowSkuName

Piano tariffario dell'app per la logica.

    "flowSkuName": {
      "type": "string",
      "defaultValue": "Standard",
      "allowedValues": [
        "Free",
        "Basic",
        "Standard",
        "Premium"
      ],
      "metadata": {
        "description": "The pricing tier for the logic app."
      }
    },


Il modello definisce i valori consentiti per il parametro (Gratuito, Basic, Standard o Premium) e assegna un valore predefinito (Standard) nel caso in cui non venga specificato alcun valore.

### hostingSkuName

Il piano tariffario del servizio app.

    "hostingSkuName": {
      "type": "string",
      "defaultValue": "S1",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "metadata": {
        "description": "Describes plan's pricing tier and instance size."
      }
    },


### hostingSkuCapacity

Dimensioni dell'istanza dell'app.

    "hostingSkuCapacity": {
      "type": "int",
      "defaultValue": 1,
      "minValue": 1,
      "metadata": {
        "description": "Describes plan's instance count"
      }
    },


Il modello definisce i valori consentiti per questo parametro (0, 1 o 2) e assegna un valore predefinito (0) nel caso in cui non venga specificato alcun valore. I valori corrispondono a piccole, medie e grandi dimensioni.

