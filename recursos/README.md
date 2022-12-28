# Recursos FHIR

En esta sección se disponen ejemplos JSON de los diferentes recuros FHIR que pudiesen ser requeridos para generar un resumen de paciente.

## Composition

```json
{
  "resourceType" : "Composition",
  "id" : "CompositionCl-PacienteX",
  "meta" : {
    "profile" : [
      "https://hl7chile.cl/fhir/ig/CoreCL/StructureDefinition/DocumentoCl"
    ]
  },
  "status" : "final",
  "type" : {
    "coding" : [
      {
        "system" : "http://loinc.org",
        "code" : "60591-5",
        "display" : "Patient Summary Document"
      }
    ]
  },
  "subject" : {
    "reference" : "Patient/11"
  },
  "date" : "2022-07-06T14:30:00+01:00",
  "author" : [
    {
      "reference" : "Practitioner/3020"
    }
  ],
  "title" : "Resumen IPS para Carlos Salas - 06 JUL 2022",
  "section" : [
    {
      "title" : "Diagnósticos",
      "code" : {
        "coding" : [
          {
            "system" : "http://loinc.org",
            "code" : "11450-4",
            "display" : "Problem list - Reported"
          }
        ]
      },
      "text" : {
        "status" : "generated",
        "div" : "<div xmlns=\"http://www.w3.org/1999/xhtml\">Asma</div>"
      },
      "entry" : [
        {
          "reference" : "Condition/1112"
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
      "text" : {
        "status" : "generated",
        "div" : "<div xmlns=\"http://www.w3.org/1999/xhtml\">Anastrozole/Cimicifuga</div>"
      },
      "entry" : [
        {
          "reference" : "MedicationStatement/354"
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
      "text" : {
        "status" : "generated",
        "div" : "<div xmlns=\"http://www.w3.org/1999/xhtml\">Allergy to penicillin, high criticality, active</div>"
      },
      "entry" : [
        {
          "reference" : "AllergyIntolerance/222"
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
      "text" : {
        "status" : "generated",
        "div" : "<div xmlns=\"http://www.w3.org/1999/xhtml\">Lista de las vacunas recibidas</div>"
      },
      "entry" : [
        {
          "reference" : "Immunization/999"
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
      "text" : {
        "status" : "generated",
        "div" : "<div xmlns=\"http://www.w3.org/1999/xhtml\">Vital signs</div>"
      },
      "entry" : [
        {
          "reference" : "Observation/123"
        }
      ]
    }
  ]
}
```