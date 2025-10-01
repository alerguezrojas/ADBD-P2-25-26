# Grado en Ingeniería Informática  
## Administración y Diseño de Base de Datos  
### Práctica 2: Modelo entidad/relación farmacia  

**Autor:** Alejandro Rodríguez Rojas  
**Correo:** alu0101317038@ull.edu.es  

---

# README – Modelo Entidad/Relación Farmacia

## Descripción general
Este documento describe el modelo entidad/relación (E/R) diseñado para la gestión de una farmacia. El modelo refleja las entidades principales del dominio, sus atributos y relaciones, así como las restricciones semánticas necesarias para representar correctamente la información.

---

## Entidades y atributos

### Medicamento
Representa cada producto que se gestiona en la farmacia.  
- **CódigoMedicamento** (PK): Identificador único. Ejemplo: `MED001`.  
- **Nombre**: Nombre comercial del medicamento. Ejemplo: `Paracetamol`.  
- **Tipo**: Forma farmacéutica. Ejemplo: `Comprimido`, `Jarabe`, `Pomada`.  
- **UnidadesStock**: Cantidad disponible en la farmacia. Ejemplo: `250`.  
- **UnidadesVendidas**: Número de unidades vendidas hasta el momento. Ejemplo: `120`.  
- **Precio**: Precio por unidad. Ejemplo: `3.50 €`.  

Especialización:  
- **MedicamentoLibre**: Se dispensa sin receta.  
- **MedicamentoConReceta**: Requiere prescripción médica.  

---

### Laboratorio
Empresa proveedora o fabricante de medicamentos.  
- **CódigoLaboratorio** (PK): Identificador único. Ejemplo: `LAB05`.  
- **Nombre**: Nombre del laboratorio. Ejemplo: `Pfizer`.  
- **Teléfono**: Teléfono de contacto. Ejemplo: `+34 922 123 456`.  
- **Dirección**: Dirección postal. Ejemplo: `Calle Mayor 10, Madrid`.  
- **Fax**: Número de fax.  
- **PersonaContacto**: Nombre de la persona responsable. Ejemplo: `Dr. López`.  

---

### Familia
Clasificación de los medicamentos según la enfermedad o dolencia a la que se aplican.  
- **CódigoFamilia** (PK): Identificador único. Ejemplo: `FAM03`.  
- **Nombre**: Nombre de la familia. Ejemplo: `Analgésicos`.  
- **Descripción**: Explicación breve. Ejemplo: `Medicamentos para aliviar el dolor`.  

---

### Cliente
Persona que compra medicamentos en la farmacia.  
- **CódigoCliente** (PK): Identificador único. Ejemplo: `CLI100`.  
- **Nombre**: Nombre completo. Ejemplo: `María Pérez`.  
- **Dirección**: Dirección postal. Ejemplo: `Calle La Laguna 15, Tenerife`.  
- **Teléfono**: Número de contacto. Ejemplo: `+34 654 123 987`.  

Subtipos:  
- **ClienteCrédito**: Cliente que paga a fin de mes.  
  - **DatosBancarios**: Información de la cuenta bancaria. Ejemplo: `ES12 3456 7890 1234`.  
- **ClienteContado**: Cliente que paga en el momento.  

---

### Compra
Registro de una transacción realizada por un cliente.  
- **IDCompra** (PK): Identificador único. Ejemplo: `COMP045`.  
- **FechaCompra**: Día de la compra. Ejemplo: `2025-10-01`.  
- **FechaPago** (opcional, atributo punteado): Solo se usa en clientes con crédito. Ejemplo: `2025-10-31`.  

---

## Relaciones y cardinalidades

### Pertenece (Familia – Medicamento)
- Cada **Medicamento** pertenece a una única **Familia** (1,1).  
- Cada **Familia** puede incluir uno o varios **Medicamentos** (1,N).  

### Fabrica/Suministra (Laboratorio – Medicamento)
- Un **Medicamento** puede estar fabricado por un laboratorio o por la farmacia (0,1).  
- Un **Laboratorio** puede fabricar o suministrar cero o muchos medicamentos (0,N).  

### Realiza (Cliente – Compra)
- Un **Cliente** puede realizar cero o varias compras (0,N).  
- Cada **Compra** corresponde exactamente a un cliente (1,1).  

### DetalleCompra (Compra – Medicamento, con atributo Cantidad)
- Una **Compra** debe incluir uno o varios medicamentos (1,N).  
- Un **Medicamento** puede aparecer en ninguna o varias compras (0,N).  
- Atributo asociado: **Cantidad** (número de unidades compradas). Ejemplo: `2`.  

---

## Restricciones semánticas

1. **Cliente exclusivo y total**  
   Todo cliente es **ClienteCrédito** o **ClienteContado**, pero nunca ambos (exclusiva y total).  

2. **Tipo de Medicamento**  
   Todo medicamento es **Libre** o **ConReceta**, de forma exclusiva y total.  

3. **Relación Medicamento – Laboratorio**  
   Un medicamento puede estar fabricado por un laboratorio o ser de producción propia, pero no ambos (restricción XOR).  

4. **Fecha de Pago**  
   - Si la compra pertenece a un **ClienteCrédito**, entonces `FechaPago ≠ NULL`.  
   - Si la compra pertenece a un **ClienteContado**, `FechaPago = FechaCompra` o `NULL` (según política de la farmacia).  

---

## Conclusión
El modelo diseñado cubre la gestión integral de medicamentos, clientes y compras en la farmacia, permitiendo identificar dependencias entre laboratorios, familias de medicamentos y modalidades de pago.
