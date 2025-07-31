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