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

El campo `resource` va el recurso FHIR siguiende la estructura específica de cada tipo de recurso.

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
    }
    //Resto del contenido
}
```

