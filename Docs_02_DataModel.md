Modelo de Datos

Tabla de Hechos: relevamiento_arboles
| Campo | Tipo | Descripción |
|-------|------|-------------|
| id_arbol | INT | PK |
| prom_altura | FLOAT | Altura en metros |
| prom_diametro | FLOAT | Diámetro tronco (cm) |
| prom_inclinacion | FLOAT | Ángulo (grados) |
| especie_id | FK | → dim_especie |
| ubicacion_id | FK | → dim_ubicacion_geo |

Tabla de Dimensiones: dim_especie
- Origen (Exótico, Nativo, Autoctonía indeterminada)
- Tipo Follaje (Árbol, Caducifolia, Conífero, etc.)
- Nombre Genérico
- Frecuencia en CABA (contexto)

Cálculos Derivados (DAX)
IMP = Ind.Mant.Prev. = ('arbolado-en-espacios-verdes'[altura_tot] + 'arbolado-en-espacios-verdes'[diametro] + 'arbolado-en-espacios-verdes'[inclinacion]) / 3

Criticidad = Estado.IMP = SWITCH(TRUE(),
    'arbolado-en-espacios-verdes'[Ind.Mant.Prev.] <= 22, "Normal",
    'arbolado-en-espacios-verdes'[Ind.Mant.Prev.] <= 30, "Excedidos",
    "Criticos"
)

Limitaciones Conocidas
⚠️ "S/D" (Sin Dato) representa 11% de registros
⚠️ Datos estáticos (no actualizados post-2023)
⚠️ Geo solo a nivel de barrio, no de calle individual
