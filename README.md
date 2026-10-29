# Ginza: ¿rinden los paneles solares lo que prometían?

Proyecto para **Ginza**, concesionaria Toyota de Rosario, que instaló paneles solares en dos de
sus sucursales. La empresa quería responder dos preguntas: **¿los paneles generan la energía que
se proyectó?** y **¿en cuánto tiempo se recupera la inversión?**

> Los datos son de la empresa, así que acá se muestra **qué hicimos y cómo**, sin cifras de
> generación, ahorro ni inversión.

**Herramientas:** Excel · Power BI · DAX

---

## Qué hicimos

1. **Relevamiento de datos.** Armamos un inventario de las fuentes de datos de la empresa para
   saber qué había, dónde y en qué formato, antes de elegir el problema.
2. **Preparación.** La información venía en planillas con formatos distintos por sucursal. La llevamos a una tabla por sucursal con una fila por mes:
   energía generada teórica y real (kWh), precio de la energía, impuestos, ahorro teórico y
   real, sus acumulados y la inversión.
3. **Tablero en Power BI** con cuatro páginas, energía y ahorro para cada sucursal:
   - **Energía:** generación real contra la teórica, mes a mes y acumulada, y un medidor de cuánto
     se generó respecto de lo que debería haberse generado a la fecha.
   - **Ahorro:** ahorro real contra el teórico, recupero de la inversión y plazo estimado de
     repago.

## Indicadores (DAX)

```DAX
Performance % = DIVIDE ( MAX ( Planta[Acumulado_EGR_kWh] ), MAX ( Planta[Acumulado_EGT_kWh] ) )
Deficit kWh   = MAX ( Planta[Acumulado_EGT_kWh] ) - MAX ( Planta[Acumulado_EGR_kWh] )
Recupero %    = DIVIDE ( MAX ( Planta[Acumulado_Total_Ahorro_Real] ), MAX ( Planta[Inversion] ) )
Payback años  = DIVIDE ( MAX ( Planta[Inversion] ),
                         AVERAGE ( Planta[Total_Real_Ahorro_Con_Impuesto] ) * 12 )
```

*EGT: energía generada teórica. EGR: energía generada real.*

## Resultado

Las dos instalaciones generaron **por debajo de lo proyectado**. El tablero permite
cuantificar cuánta energía y cuánto ahorro faltaron en cada sucursal y tener una
estimación del plazo de recupero para comparar con lo que se había prometido al invertir.

## Qué mejoraría

El payback está calculado en pesos nominales, con el ahorro mensual promedio. En un contexto de
inflación y tarifas cambiantes eso puede distorsionar bastante el plazo. Una versión más sólida
expresaría ahorro e inversión en moneda constante (o en dólares) y descontaría los flujos
futuros.

---

*Proyecto grupal para un cliente real en la materia Laboratorio I, Licenciatura en Ciencia de
Datos, UCA Rosario (2026). Con el mismo cliente seguimos en Laboratorio II, ahora con el análisis
del consumo de agua.*
