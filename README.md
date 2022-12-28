# Construyendo un Resumen de Paciente

La forma correcta de generar un resumen de paciente, es mediante la construcción de un Bundle de tipo `document`.

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

El campo `fullUrl` URI para el recurso, se sugiere emplear uuid por ejemplo `urn:uuid:53fefa32-fcbb-4ff8-8a92-55ee120877b7`. Tener en cuenta que es sensible a mayúsculas.

El campo `resource` 