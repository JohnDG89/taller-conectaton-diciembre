# Construyendo un Resumen de Paciente

## Teoría inicial

### Bundle

La forma correcta de generar un resumen de paciente, es mediante la construcción de un `Bundle` de tipo `document`.

Lo primero que veremos es la estructura de un bundle, la cual se representa así:

```json
{
  "resourceType": "Bundle",
  "id": "id-bundle",
  "type": "document",
  "entry": []
}
```

Dentro del array `entry` debemos ingresar todos nuestros recursos que componen el resumen del paciente.

Cada `entry` ingresada debe contener la siguiente estructura:

```json
{
  "fullUrl": "",
  "resource": {}
}
```

El campo `fullUrl` es una URI para el recurso, se sugiere emplear uuid por ejemplo `urn:uuid:53fefa32-fcbb-4ff8-8a92-55ee120877b7`. Tener en cuenta que es sensible a mayúsculas.

### Creación de un recurso

El campo `resource` visto en el punto anterior, va el recurso FHIR siguiende la estructura específica de cada tipo de recurso.

```json
{
  "resourceType": "",
  "id": "",
  "meta": {},
}
```

El recurso debe comenzar con el `resourceType` indicando a qué tipo de recurso se refiere. 

El campo `id` debe coincidir con el empleado en la fullUrl, en este caso podría ser `53fefa32-fcbb-4ff8-8a92-55ee120877b7` si es que empleamos como fullUrl de este recurso `urn:uuid:53fefa32-fcbb-4ff8-8a92-55ee120877b7`.

Para visualizar mejor lo anterior, se muestra un ejemplo de cómo iría un recurso completo a insertar en el campo `entry` de nuestro bundle.

```json
{
  "fullUrl" : "urn:uuid:7685713c-e29e-4a75-8a90-45be7ba3be94",
  "resource" : {
    "resourceType" : "Patient",
    "id" : "7685713c-e29e-4a75-8a90-45be7ba3be94",
      //Resto del contenido del recurso Patient
  }
}
```

### Referenciando recursos dentro del bundle

Cuando se construye un resumen de paciente, emplearemos referencias entre los recursos que se han añadido al `Bundle`.

Veremos el ejemplo cuando usamos uuid como fullUrl, podemos referencias esto de dos formas:

1. Referenciando al id del recurso: 

    ```json
    {
      "reference": "Patient/7685713c-e29e-4a75-8a90-45be7ba3be94"
    }
    ```

2. Referenciando a la fullUrl del `entry` ingresado en nuestro `Bundle`:

    ```json
    {
      "reference": "urn:uuid:7685713c-e29e-4a75-8a90-45be7ba3be94"
    }
    ```

## Resumen de paciente

Un resumen de paciente en el contexto FHIR se representa mediante la generación de un recurso  `Bundle` de tipo `document`, debe contener en la sección `entry` un recurso `Composition` el cual es el que referencia los recursos y los ordena por secciones.

### Composition

El recurso `Composition` debe llevar en el campo `type` el código loinc asociado a un resumen de paciente, además de tener un `status` del tipo *final*:

```json
{
  "type" : {
    "coding" : [
      {
        "system" : "http://loinc.org",
        "code" : "60591-5",
        "display" : "Patient summary Document"
      }
    ]
  },
  "status": "final"
}
```

Debe referenciar al paciente a quien corresponde el resumen.

```json
{
  "subject": {
    "reference" : "Patient/7685713c-e29e-4a75-8a90-45be7ba3be94"
  }
}
```

Debe llevar una fecha de edición, un título y el autor/es del resumen.

```json
{
  "date": "2022-12-28T14:30:00+01:00",
  "author" : [
    {
      "reference" : "Practitioner/uuid-practitioner"
    }
  ],
  "title" : "Resumen IPS para el paciente X",
}
```

Debe contener una sección llamada `section`, en donde se agrupan los diferentes datos clínicos del paciente, las cuales pueden ser:
- Diagnósticos
- Medicamentos
- Alergias y Reacciones Adversas
- Antecedentes de Embarazo
- Signos Vitales Medidos

**Importante**: Debe contener al menos una sección.

Cada una de las secciones debe tener un título, si lleva un code este deber ser el código loinc correspondiente a cada una de las secciones, debe llevar al menos un elemento bajo la sección `entry` la cual corresponde a referencias a recuros FHIR del tipo especificado para cada sección:

```json
{
  "section" : [
    {
      "title": "Diagnósticos",
      "code" : {
        "coding" : [
          {
            "system" : "http://loinc.org",
            "code" : "11450-4",
            "display" : "Problem list - Reported"
          }
        ]
      },
      "entry" : [
        {
          "reference" : "Condition/id-condition"
        }
      ]
    },
    {
      "title" : "Medicamentos",
      "code" : {
        "coding" : [
          {
            "system" : "http://loinc.org",
            "code" : "10160-0",
            "display" : "Hx of Medication use"
          }
        ]
      },
      "entry" : [
        {
          "reference" : "MedicationStatement/id-medicationstatement"
        }
      ]
    },
    {
      "title" : "Alergias",
      "code" : {
        "coding" : [
          {
            "system" : "http://loinc.org",
            "code" : "48765-2",
            "display" : "Allergies and adverse reactions Document"
          }
        ]
      },
      "entry" : [
        {
          "reference" : "AllergyIntolerance/id-allergyintolerance"
        }
      ]
    },
    {
      "title" : "Vacunas",
      "code" : {
        "coding" : [
          {
            "system" : "http://loinc.org",
            "code" : "11369-6",
            "display" : "Hx of Immunization"
          }
        ]
      },
      "entry" : [
        {
          "reference" : "Immunization/id-immunization"
        }
      ]
    },
    {
      "title" : "Antecedentes de Embarazo",
      "code" : {
        "coding" : [
          {
            "system" : "http://loinc.org",
            "code" : "10162-6",
            "display" : "Hx of pregnancies Narrative"
          }
        ]
      },
      "entry" : [
        {
          "reference" : "Observation/id-observation"
        }
      ]
    },
    {
      "title" : "Signos Vitales y Mediciones Fisiológicas",
      "code" : {
        "coding" : [
          {
            "system" : "http://loinc.org",
            "code" : "8716-3",
            "display" : "Vital signs"
          }
        ]
      },
      "entry" : [
        {
          "reference" : "Observation/id-observation"
        }
      ]
    }
  ]
}
```