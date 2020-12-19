# PoC Red Hat Decision Manager
_El objetivo principal de la PoC es probar el ciclo completo de toma de decisiones automatizado, el ciclo debe ser realizado desde la invocación de un sistema origen a la solución Red Hat Decision Manager,  como respuesta al llamado la solución debe entregar una respuesta basada en las reglas de negocio implementadas y finalmente la respuesta debe permitir a un sistema destino la ejecución de un proceso de negocio._

## Diagnóstico de Diabetes 🥼

_Dada la necesidad de automatizar la toma de decisiones para el diagnóstco de Diabetes por parte de Keralty, se diseñó un flujo donde intervienen las siguientes tomas de decisiones: Tamizaje, Test de Findrisk, Diagnóstico por Glicemia en Ayunas, Glicemia a cualquier hora, Hb1Ac, PTOG y fnalmente Diagnóstico de Prediabetes y Diabetes._

Para más detalle este es el diagrama: ![Business Process](https://github.com/mirkhala/kt-diabetes/blob/master/flow/flujo-proceso-diabetes.png?raw=true)


### ¿Dónde probar las reglas? 📋

_Una vez instalado Red Hat Decision Manager e importado el proyecto, ingresar al kie-server para probar las reglas bajo **KIE session assets**._

_**Ejemplo de URL del kie-server:**_

```
http://localhost:8080/kie-server/docs
```

### ¿Cómo probar las reglas? 🔧

_Se prueban mediante un json dependiendo de la regla a probar. Por ejemplo, para **Findrisk Alto**:_

```
{  
    "lookup":"default-stateless-ksession",
    "commands":[  
       {  
          "insert":{  
             "object":{  
                "com.myspace.rhdm7_keralty.Paciente":{ 
                   "edad": 70,
                   "imc": 35,
                   "genero": "mujer",
                   "perimetroAbdominal": 90,
                   "actividadFisica": false,
                   "alimentacionSaludable": false,
                   "medicamentosHipertension": true,
                   "histGlucosaAlta": true,
                   "herenciaDiabetesGradoUno": true,
                   "herenciaDiabetesGradoDos": true
                }
             },
             "out-identifier":"findrisk"
          }
       },
       {  
          "start-process":{  
             "processId":"rhdm7_keralty.EvaluacionFindrisk",
             "parameter":[  
 
             ],
             "out-identifier":null
          }
       }
    ]
 }
```

_o para **Findrisk Bajo**:_

```
{  
    "lookup":"default-stateless-ksession",
    "commands":[  
       {  
          "insert":{  
             "object":{  
                "com.myspace.rhdm7_keralty.Paciente":{ 
                   "edad": 38,
                   "imc": 22,
                   "genero": "mujer",
                   "perimetroAbdominal": 60,
                   "actividadFisica": true,
                   "alimentacionSaludable": true,
                   "medicamentosHipertension": false,
                   "histGlucosaAlta": false,
                   "herenciaDiabetesGradoUno": false,
                   "herenciaDiabetesGradoDos": false
                }
             },
             "out-identifier":"revision"
          }
       },
       {  
          "start-process":{  
             "processId":"rhdm7_keralty.EvaluacionFindrisk",
             "parameter":[  
 
             ],
             "out-identifier":null
          }
       }
    ]
 }
```

Para más detalle en [test](https://github.com/mirkahala/kt-diabetes/-/tree/master/test) puedes ver algunos ejemplos.

## ¿Qué se espera? ⚙️

_Se espera que una vez ejecutado el código, e invoque las reglas con los **facts** enviados, evalúe las condiciones y muestre en **mensaje** la acción tomada._

## Construido con 🛠️

_Todas las reglas fueron creadas a manera de prueba, con distintos **Assets**. Para más información visita:_

* [Red Hat Decision Manager](https://access.redhat.com/documentation/en-us/red_hat_decision_manager/7.8/) - Motor de Reglas de Negocio.

## Autores ✒️

_Los participantes de la PoC fueron:_

* **Juan Carlos Naranjo** - *Toma de Requisitos* - [jcarlos@redhat.com](jcarlos@redhat.com) - Account Solution Architect
* **Magda Martínez** - *Diseño de Flujo y Reglas de Negocio* - [magda.martinez@redhat.com](magda.martinez@redhat.com) - Specialist Solution Architect

## Agradecimientos Especiales 🎁
* **Diego Torres Fuerte** - *Drools* - [dtorresf@redhat.com](dtorresf@redhat.com) - Middleware Delivery

---
⌨️ Creado por [Red Hat.](https://www.redhat.com/)