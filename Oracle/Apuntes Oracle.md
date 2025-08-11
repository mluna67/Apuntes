# Oracle

## Crecion de una tabla nueva
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

## Uso de Vairables
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