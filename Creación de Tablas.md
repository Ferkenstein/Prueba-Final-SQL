# Creación de Tablas

## Tabla Programa

La tabla Programa cuenta con un id y el nombre del programa, el cual solo acepta el nombre Java o Android.

```SQL
CREATE TABLE Programa (
idPrograma Integer Primary Key Not Null,
nombrePrograma Varchar2(50) Not Null
);

ALTER TABLE Programa
ADD CONSTRAINT check_nombrePrograma
  CHECK (nombrePrograma IN ('Java', 'Android'));
```
## Tabla Prueba

La tabla Prueba cuenta con un id, un idPrograma, el nombre de la prueba, la descripción, la unidad a la que pertenece, el autor y la fecha de creación de esta.

```SQL

CREATE TABLE Prueba (
idPrueba Integer Primary Key Not Null,
idPrograma Integer Not Null,
nombrePrueba Varchar2(50) Not Null,
descripcionPrueba Varchar2(50) Not Null,
unidadPrueba Integer Not Null,
autorPrueba Varchar2(50) Not Null,
fechaCreacion Date Not Null
);

ALTER TABLE Prueba
ADD FOREIGN KEY (idPrograma) 
REFERENCES Programa(idPrograma);
```
  
## Tabla Pregunta

La tabla Pregunta cuenta con un identificador único, un enunciado y un puntaje asociado.

``` SQL
CREATE TABLE Pregunta (
idPregunta Integer Primary Key Not Null,
idPrueba Integer Not Null,
enunciadoPregunta Varchar2(50) Not Null,
puntajePregunta Integer Not Null
);

ALTER TABLE Pregunta 
ADD FOREIGN KEY (idPrueba) 
REFERENCES Prueba(idPrueba);

```

## Tabla Alternativa

La tabla alternativa cuenta con un identificador único, una descripción, y un valor lógico que señala si es o no una respuesta correcta, más un valor que indica el porcentaje que aporta esta alternativa (de ser una respuesta correcta) al puntaje total de la pregunta.

```SQL

CREATE TABLE Alternativa (
idAlternativa Integer Primary Key Not Null,
idPregunta Integer Not Null,
respuestaAlternativa Varchar2(50) Not Null,
esCorrecta Char Not Null,
porcentaje Integer Not Null
);

ALTER TABLE Alternativa
ADD CONSTRAINT check_esCorrecta
  CHECK (esCorrecta IN ('S', 'N'));

ALTER TABLE Alternativa ADD FOREIGN KEY (idPregunta) REFERENCES Pregunta(idPregunta);

```

## Tabla Alumno

La tabla alumno cuenta con un id y el nombre del Alumno.

```SQL
CREATE TABLE Alumno (
idAlumno Integer Primary Key Not Null,
nombreAlumno Varchar(50) Not Null
);
```

## Tabla Respuesta Alumno

La tabla RespuestaAlumno cuenta con un id, el id de la tabla alumno, el id de la tabla prueba, el id de la tabla pregunta y el id de la tabla alternativa.

```SQL
CREATE TABLE respuestaAlumno (
idrespuestaAlumno Integer Primary Key Not Null, 
idAlumno Integer Not Null,
idPrueba Integer Not Null,
idPregunta Integer Not Null,
idAlternativa Integer Not Null
);

ALTER TABLE respuestaAlumno ADD FOREIGN KEY (idAlumno) REFERENCES Alumno(idAlumno);
ALTER TABLE respuestaAlumno ADD FOREIGN KEY (idPrueba) REFERENCES Prueba(idPrueba);
ALTER TABLE respuestaAlumno ADD FOREIGN KEY (idAlternativa) REFERENCES Alternativa(idAlternativa);
ALTER TABLE respuestaAlumno ADD FOREIGN KEY (idPregunta) REFERENCES Pregunta(idPregunta);
```

## Tabla AlumnoPrograma

La tabla AlumnoPrograma cuenta con un id, el id de la tabla programa y el id de la tabla alumno.

```SQL
CREATE TABLE ALUMNOPROGRAMA (
idAlumnoPrograma Integer Primary Key Not Null,
idPrograma Integer Not Null,
idAlumno Integer Not Null
);

ALTER TABLE ALUMNOPROGRAMA
ADD FOREIGN KEY (idPrograma) 
REFERENCES Programa(idPrograma);

ALTER TABLE ALUMNOPROGRAMA
ADD FOREIGN KEY (idAlumno) 
REFERENCES Alumno(idAlumno);
```