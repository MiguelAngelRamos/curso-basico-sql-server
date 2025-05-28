Url [https://dbdiagram.io]

```dbml
Table Instructores {
  Id int [pk, increment]
  Nombre nvarchar(100) [not null]
  Email nvarchar(100) [not null, unique]
}

Table Cursos {
  Id int [pk, increment]
  Titulo nvarchar(100) [not null]
  Descripcion nvarchar(max)
  InstructorId int [ref: > Instructores.Id]
}
```