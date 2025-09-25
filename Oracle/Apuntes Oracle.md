# Oracle
## Conexión
En oracle cada conexión nueva es un esquema diferente, por lo tanto si tengo el Usuario "LUNA" no va a poder ver ni gestionar las tablas de "sys", para que un usuario pueda acceder a las tabla de otro se debe dar el permiso

``` sql
GRANT SELECT, INSERT, UPDATE, DELETE ON tabla_x TO otro_usuario;
``` 
Adicional si el rol del usuario no es dba, no va a dejar crear vistas u otras cosas, por lo que desde el usuario sys se le deben otorgar esos permisos
``` sql
GRANT CREATE VIEW TO LUNA;
```

y el otro usuario accedería poniendo el propietario antes de la tabla
``` sql
SELECT * FROM LUNA.nombre_tabla;
```
O se puede crear un sinonimo para no llamarlo así
``` sql
CREATE SYNONYM tabla_x FOR VENTAS_APP.tabla_x;
```

## Creación de una tabla nueva
``` sql
CREATE TABLE TB_ARTICULOS
            (CODIGO_AR NUMBER(5,0)
            ,DESCRIPCION_AR VARCHAR2(200)
            ,MARCA_AR VARCHAR2(30)
            ,CODIGO_ME NUMBER(3,0)
            ,CODIGO_CA NUMBER(4,0)
            ,FECHA_ING DATE
            ,STOCK_ACTUAL DECIMAL(10,2)
            ); 
```
## Modificar una columan ya creada
``` sql         
ALTER TABLE TB_MEDIDAS MODIFY (CODIGO_MD NOT NULL)
ALTER TABLE TB_MEDIDAS MODIFY DESCRIPCION_ME VARCHAR(30)
ALTER TABLE TB_ARTICULOS MODIFY CODIGO_AR NUMBER(6,0) DEFAULT 0;
```
## Eliminar una columan ya creada
``` sql   
ALTER TABLE TB_ARTICULOS DROP COLUMN MARCA_AR;
```
## Agregar una columan a una tabla
``` sql 
ALTER TABLE TB_ARTICULOS ADD MARCA_AR VARCHAR2(30);
```
## Columnas Virtuales
Son campos calculados, no ocupan espacio físico
``` sql
CREATE TABLE productos (
    producto_id NUMBER,
    precio_unitario NUMBER,
    cantidad NUMBER,
    total_venta AS (precio_unitario * cantidad)
);
``` 
## Agregar una llave primaria
``` sql 
ALTER TABLE TB_ARTICULOS ADD PRIMARY KEY (CODIGO_AR)
```

## Agregar una llave foranea
``` sql 
ALTER TABLE TB_ARTICULOS
ADD CONSTRAINT FK_02 
FOREIGN KEY (CODIGO_CA) 
REFERENCES TB_CATEGORIAS (CODIGO_CA);
```

## Identity
``` sql 
CREATE TABLE TB_ALMACENES(
    CODIGO_AL NUMBER(2,0) GENERATED ALWAYS AS IDENTITY, 
    DESCRIPTION_AL VARCHAR2(30)
)

```
## Isertar un nuevo dato
``` sql 
INSERT INTO TB_ARTICULOS
            (CODIGO_AR
            ,DESCRIPCION_AR
            ,MARCA_AR
            ,CODIGO_ME
            ,CODIGO_CA) 
      VALUES(1
            ,'COMPUTADOR'
            ,'ASUS'
            ,1
            ,3
            );
```

## PLSQL

## Bloques anonimos
``` sql 
BEGIN
    dbms_output.put_line('Hola mundo');
END;
```

## Uso de Variables
Para imprimir una variable debo usar el ||
``` sql 
DECLARE

    v_num NUMBER(2) := 10;
    v_cadena VARCHAR(10) := 'Marco';
    v_fecha DATE := SYSDATE;

BEGIN
    dbms_output.put_line('El valor de v_num es ' || v_num);
    dbms_output.put_line('El valor de v_cadena es ' || v_cadena);
    dbms_output.put_line('El valor de v_fecha es ' || v_fecha);

END;
```
## Pedir datos al usuario
Lo que ponemos despues del := & es para definir como verá el usuario la solicitud de ese dato
``` sql
DECLARE

    v_opl NUMBER(2) := &operando1;
    v_op2 NUMBER(2) := &operando2;
    v_sum NUMBER(3);


BEGIN
    v_sum := v_opl + v_op2;
    dbms_output.put_line(v_opl ||' + ' || v_op2 || ' = ' || v_sum);

END;
```
## Operadores
Toda operación concatenada debe ir dentro de paréntesis 
``` sql
DECLARE

    v_nl NUMBER(2) := 10;
    v_n2 NUMBER(2) := 2;


BEGIN
    
    dbms_output.put_line('SUMA ' || (v_nl+v_n2));
    dbms_output.put_line('RESTA ' || (v_nl-v_n2));
    dbms_output.put_line('MULTIPLICACION ' || (v_nl*v_n2));
    dbms_output.put_line('DIVISION ' || (v_nl/v_n2));
    dbms_output.put_line('POTENCIA ' || (v_nl**v_n2));

END;

```

## If

``` sql

DECLARE
    v_n1 Number(2):= 10;
    v_n2 Number(2):= 4;
    v_n3 Number(2):= 3;
BEGIN

    IF v_n1 >= v_n2 AND v_n1>= v_n3
        THEN
            dbms_output.put_line(v_n1 || ' es el mayor');
        ELSIF v_n2>= v_n3
            THEN
                dbms_output.put_line(v_n2 || ' es el mayor');
        ELSE
            dbms_output.put_line(v_n3 || ' es el mayor');
        END IF;

END;
```

## Case
Es similar al de SQL Server
``` sql
DECLARE
    v_dia NUMBER := 3;
    v_nombre_dia VARCHAR2(20);
BEGIN
    v_nombre_dia := CASE v_dia
                        WHEN 1 THEN 'Lunes'
                        WHEN 2 THEN 'Martes'
                        WHEN 3 THEN 'Miércoles'
                        ELSE 'Desconocido'
                    END;
    DBMS_OUTPUT.PUT_LINE('Día: ' || v_nombre_dia);
END;

```

## Bucles
Son infinitos si no se les pone la clausula ``` exit ```
``` sql
DECLARE
    v_contador NUMBER := 1;
BEGIN
    LOOP
        DBMS_OUTPUT.PUT_LINE('Iteración: ' || v_contador);
        v_contador := v_contador + 1;
        EXIT WHEN v_contador > 5;
    END LOOP;
END;
```

## While
Funciona con un loop interno
``` sql
DECLARE
    v_num NUMBER := 1;
BEGIN
    WHILE v_num <= 5 LOOP
        DBMS_OUTPUT.PUT_LINE('Número: ' || v_num);
        v_num := v_num + 1;
    END LOOP;
END;
```

## FOR
Funciona con un loop interno y la secuencia se define con ..
``` sql
BEGIN
    FOR i IN 1..5 LOOP
        DBMS_OUTPUT.PUT_LINE('Valor de i: ' || i);
    END LOOP;
END;
```
Tambien se puede hacer descendente
``` sql
BEGIN
    FOR i IN REVERSE 5..1 LOOP
        DBMS_OUTPUT.PUT_LINE('Valor de i: ' || i);
    END LOOP;
END;
```

## Funciones
Recibe cero o más parámetros, realiza un conjunto de operaciones y devuelve un único valor.
``` sql
CREATE OR REPLACE FUNCTION nombre_funcion (param1 DATATYPE, param2 DATATYPE)
RETURN tipo_retorno
IS
    -- Declaración de variables locales
BEGIN
    -- Lógica de la función
    RETURN valor;
END;
```

## Tipos
Es como un molde o plantilla que define qué forma y qué tipo de datos puede contener una variable, parámetro o columna.
Pueden ser simples, compuestos o orientados a objetos.

### Simples
Representan un único valor y no contienen otros datos dentro (lo que llamamos variables).
``` sql
DECLARE
    v_nombre VARCHAR2(50);
    v_edad NUMBER(3);
BEGIN
    v_nombre := 'Marco';
    v_edad := 35;
    DBMS_OUTPUT.PUT_LINE(v_nombre || ' tiene ' || v_edad || ' años.');
END;
```
### Compuestos
Contienen varios valores a la vez pueden ser:
#### Record
Similar a una fila de una tabla.
``` sql
DECLARE
    TYPE t_empleado IS RECORD (
        id     NUMBER,
        nombre VARCHAR2(50),
        sueldo NUMBER
    );
    v_emp t_empleado;
BEGIN
    v_emp.id := 1;
    v_emp.nombre := 'Marco';
    v_emp.sueldo := 2500;
    DBMS_OUTPUT.PUT_LINE(v_emp.nombre || ' gana ' || v_emp.sueldo);
END;
```
#### Colecciones
Similares a un Array
este es un ejemplo de un VARRAY, que es un Array con tamaño fijo
``` sql
DECLARE
    TYPE t_lista IS VARRAY(5) OF VARCHAR2(50);
    v_frutas t_lista := t_lista('Manzana', 'Pera', 'Uva');
BEGIN
    FOR i IN 1..v_frutas.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(v_frutas(i));
    END LOOP;
END;
```

### Tipos definidos por el usuario (UDT)
Se comportan como clases en programación.
</br>
El siguiente sería un ejemplo de como usarla similar a como se usa en SQL Server:   
``` sql
CREATE OR REPLACE TYPE persona_t AS OBJECT (
    id NUMBER,
    nombre VARCHAR2(100)
);
``` 
se crea un tipo colección basado en el objeto
``` sql
-- Tipo colección de personas
CREATE OR REPLACE TYPE personas_tab_t AS TABLE OF persona_t;
``` 
Para usarlas en PL sería mas o menos así:
``` sql
DECLARE
    v_personas personas_tab_t := personas_tab_t();
BEGIN
    -- Agregar registros
    v_personas.EXTEND(3);
    v_personas(1) := persona_t(1, 'Marco Aurelio');
    v_personas(2) := persona_t(2, 'Ana María');
    v_personas(3) := persona_t(3, 'Carlos López');

    -- Mostrar en consola
    FOR i IN 1 .. v_personas.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(v_personas(i).id || ' - ' || v_personas(i).nombre);
    END LOOP;
END;
```

Para hacer un select como en sql tocaría definir primero una función que cree los objetos
``` sql
CREATE OR REPLACE FUNCTION obtener_personas
RETURN personas_tab_t PIPELINED
AS
BEGIN
    PIPE ROW(persona_t(1, 'Marco Aurelio'));
    PIPE ROW(persona_t(2, 'Ana María'));
    PIPE ROW(persona_t(3, 'Carlos López'));
    RETURN;
END;
```

y el select sería así:
``` sql 
SELECT *
FROM TABLE(obtener_personas);
``` 

#### Ejemplo de uso de tipos similar a sql server
Se crea un procedimiento que espere como parametro una tabla y la inserte directente en la tabla de la base de datos que tiene la siguiente estructura:

``` sql
CREATE TABLE detalle_factura (
    id_detalle NUMBER GENERATED BY DEFAULT AS IDENTITY,
    producto   VARCHAR2(100),
    cantidad   NUMBER,
    precio     NUMBER
);
``` 

ENTONCES LOS PASOS SON LOS SIGUIENTES:
1. Se crea un tipo de objeto

``` sql
CREATE OR REPLACE TYPE t_detalle_factura AS OBJECT (
    producto   VARCHAR2(100),
    cantidad   NUMBER,
    precio     NUMBER
);
```

2. Se crea un tipo de tabla que es mas una colección de este tipo de objetos

``` sql
CREATE OR REPLACE TYPE t_tabla_detalle_factura AS TABLE OF t_detalle_factura;
```

3. El procedimiento lo usaría así:
``` sql
CREATE OR REPLACE PROCEDURE insertar_detalle_factura(
    p_detalle IN t_tabla_detalle_factura
) AS
BEGIN
    FORALL i IN 1 .. p_detalle.COUNT --hace inserciones masivas sin necesidad de bucles lento
        INSERT INTO detalle_factura
                   (producto
                   ,cantidad
                   ,precio)
            VALUES (p_detalle(i).producto
                   ,p_detalle(i).cantidad
                   ,p_detalle(i).precio
                   );
    END;
```

4. Se llama al procedimiento de la siguiente manera

``` sql
DECLARE
    v_detalles t_tabla_detalle_factura := t_tabla_detalle_factura();
BEGIN
    -- Agregamos 2 registros a la colección
    v_detalles.EXTEND(2);
    v_detalles(1) := t_detalle_factura('Laptop', 2, 1500);
    v_detalles(2) := t_detalle_factura('Mouse', 5, 20);

    -- Llamamos al procedimiento
    insertar_detalle_factura(v_detalles);
    
    SELECT * FROM DETALLE_FACTURA;
END;
```

Y en .NET se haría algo así
``` c#
using Oracle.ManagedDataAccess.Client;
using System;

class Program
{
    static void Main()
    {
        using (var conn = new OracleConnection("User Id=EVIDENCIASPRU;Password=1234;Data Source=192.168.0.241:1521/JARPRU;"))
        {
            conn.Open();

            using (var cmd = new OracleCommand("InsertarDetalleFactura", conn))
            {
                cmd.CommandType = System.Data.CommandType.StoredProcedure;

                // ID de la factura
                cmd.Parameters.Add("p_factura_id", OracleDbType.Int32).Value = 1001;

                // Declarar arrays
                string[] productos = { "Teclado", "Mouse", "Monitor" };
                decimal[] cantidades = { 2, 1, 1 };
                decimal[] precios = { 50.0m, 25.0m, 300.0m };

                // Crear el array de objetos
                var detalles = new OracleParameter("p_detalles", OracleDbType.Array);
                detalles.UdtTypeName = "EVIDENCIASPRU.DETALLEFACTURATABLA"; // Tipo en Oracle con schema
                detalles.Value = new DetalleFacturaObj[]
                {
                    new DetalleFacturaObj { producto = "Teclado", cantidad = 2, precio = 50 },
                    new DetalleFacturaObj { producto = "Mouse", cantidad = 1, precio = 25 },
                    new DetalleFacturaObj { producto = "Monitor", cantidad = 1, precio = 300 }
                };

                cmd.Parameters.Add(detalles);

                cmd.ExecuteNonQuery();
            }
        }
    }
}

// Clase que representa el UDT
public class DetalleFacturaObj
{
    public string producto { get; set; }
    public decimal cantidad { get; set; }
    public decimal precio { get; set; }
}

```
## Nested Table
Es similar a un VARRAY pero no tiene limites de tamaño, No garantiza el orden y en la BD se almacena como una tabla separada (heap) con relación al padre

``` sql
TYPE EMPLEADO IS TABLE OF EMPLOYEES%ROWTYPE;

CREATE TABLE EMPLES
(
CODIGO NUMBER,
DIRECCION VARCHAR2(100),
DATOS EMPLEADO
)
NESTED TABLE DATOS STORE AS TABLA_DATOS;
/

DESC EMPLES;

SELECT * FROM TAB;
``` 

## Secuencias
Llevan un control sobre los campos incrementales y se definen de la siguiente forma:

``` sql
CREATE SEQUENCE nombre_secuencia
    START WITH 1       -- Valor inicial
    INCREMENT BY 1     -- Paso de incremento
    NOCACHE            -- No guarda valores en memoria (opcional)
    NOCYCLE;           -- No reinicia cuando llega al máximo
```
Y la secuencia se utilizaría de la siguiente forma:
``` sql
INSERT INTO empleados (empleado_id, nombre)
VALUES (seq_empleados.NEXTVAL, 'Juan Pérez');
```
.NEXTVAL ➡ obtiene el siguiente valor disponible.</BR>
.CURRVAL ➡ devuelve el último valor usado en la sesión actual.

## Triggers
Se ejecuta automáticamente cuando ocurre un evento en una tabla, vista o a nivel de esquema/base de datos.

```sql
CREATE OR REPLACE TRIGGER trg_auditar_insert_producto
AFTER INSERT ON productos
FOR EACH ROW
BEGIN
    INSERT INTO auditoria_productos (usuario, fecha_evento, operacion, producto_id)
    VALUES (USER, SYSDATE, 'INSERT', :NEW.id_producto);
END;
```

### En eventos DML
``` BEFORE INSERT ``` → Antes de insertar un registro.</BR>
``` AFTER INSERT ``` → Después de insertar un registro.</BR>
``` BEFORE UPDATE ``` → Antes de actualizar.</BR>
``` AFTER UPDATE ``` → Después de actualizar.</BR>
``` BEFORE DELETE ``` → Antes de eliminar.</BR>
``` AFTER DELETE ``` → Después de eliminar.</BR>

### En vistas
En vez de (``` INSTEAD OF ```) → usados en vistas.
### En esquemas
Antes o después de un ``` CREATE TABLE ```
### En Base de datos
ej: login, logout, startup, shutdown

### Tambien se pueden combinar las secuencias y los disparadores

 ``` sql
CREATE OR REPLACE TRIGGER trg_empleados_id
    BEFORE INSERT ON empleados
    FOR EACH ROW
        BEGIN
            :NEW.empleado_id := seq_empleados.NEXTVAL;
        END;
```

Y el insert se haría solo de la siguiente forma
``` sql
INSERT INTO empleados (nombre) VALUES ('María López');
```
## Vistas
Misma lógica de una vista en sql server
``` sql
CREATE OR REPLACE VIEW Vista_Facturas_Con_Detalle AS
SELECT 
    f.FacturaID,
    f.Fecha,
    f.Cliente,
    SUM(d.Cantidad * d.Precio) AS TotalCalculado
FROM Facturas f
JOIN DetalleFactura d ON f.FacturaID = d.FacturaID
GROUP BY f.FacturaID, f.Fecha, f.Cliente;
```

## Vistas Materializadas
Misma lógica de las vistas normales pero los datos si se almacenan en la base de datos y se puede refrescar periodicamente o manualmente
``` sql
CREATE MATERIALIZED VIEW ventas_mensuales
 BUILD IMMEDIATE --construye la vista al momento de crearla.
 REFRESH COMPLETE --al actualizar, borra y recalcula todo
 START WITH SYSDATE
 NEXT SYSDATE + 30 --refresca cada 30 días (puede ser minutos, horas, etc.).
 AS
    SELECT producto
          ,SUM(cantidad) AS total_vendido
      FROM ventas
  GROUP BY producto;
```

## Bulk Collections
Los Bulk Collections son una característica de Oracle PL/SQL que permite cargar o procesar múltiples filas a la vez (en bloque), en lugar de hacerlo fila por fila.

``` sql
DECLARE
    TYPE t_employees IS TABLE OF employees%ROWTYPE;
    l_employees t_employees;
BEGIN
    SELECT * BULK COLLECT INTO l_employees
    FROM employees
    WHERE department_id = 10;

    -- Puedes recorrer el array
    FOR i IN 1 .. l_employees.COUNT LOOP
        DBMS_OUTPUT.PUT_LINE(l_employees(i).first_name);
    END LOOP;
END;

```
