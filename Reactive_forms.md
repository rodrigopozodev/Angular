## Importación

importamos `ReactiveFormsModule` para poder usar los formularios reactivos:

```ts
import { Component } from "@angular/core";
import { ReactiveFormsModule } from "@angular/forms";

@Component({
  selector: "app-login",
  imports: [ReactiveFormsModule],
  templateUrl: "./login.html",
  styleUrl: "./login.css",
})
export class Login {}
```

## Variables

1. Dentro de la `clase` creamos una serie de `variables`:

- `FormGroup` para el formulario.
- `FormControl` para sus campos.

```ts
export class Login {
  loginForm: FormGroup; //variable para el formulario de inicio de sesión
  email: FormControl; //variable para el control de email
}
```

- Se actualizara este import (en caso de que no se actualice solo lo actualizaremos nosotros):

```ts
import { FormControl, FormGroup, ReactiveFormsModule } from "@angular/forms";
```

2. Inicializamos las variables en el constructor.

```ts
  constructor() {
    this.email = new FormControl();
    this.password = new FormControl();
}
```

3. Entre los parentesis ponemos el valor inicial que queremos que tenga.

```ts
  constructor() {
    this.email = new FormControl('');
    this.password = new FormControl('');
}
```

- Será una cadena vacía por vamos a empezar con un formulario y lo habitual es que los usarios tengan los campos vacíos.
- Pero si quisiéramos cualquier otro valor, pondriamos entre las comillas lo que quisieramos.

4. Ya tenemos los campos inicializados. Ahora inicializaremos el formulario.

```ts
this.loginForm = new FormGroup({});
```

- Esto necesita un objeto. Este objeto va a ser el formulario es decir el bojeto que contenga los campos del formulario.
- En este objeto llamamos a las propiedades del formulario.

```ts
this.loginForm = new FormGroup({
  email: this.email,
  password: this.password,
});
```

- Ya tenemos inicializado el formulario reactivo.
- Esto es realmente con lo que vamos a trabajar.

## Coidgo del componeten ts completo hasta ahora:

```ts
import { Component } from "@angular/core";
import { FormControl, FormGroup, ReactiveFormsModule } from "@angular/forms";

@Component({
  selector: "app-login",
  imports: [ReactiveFormsModule],
  templateUrl: "./login.html",
  styleUrl: "./login.css",
})
export class Login {
  loginForm: FormGroup; //variable para el formulario de inicio de sesión
  email: FormControl; //variable para el control de email
  password: FormControl; //variable para el control de contraseña

  constructor() {
    this.email = new FormControl("");
    this.password = new FormControl("");

    this.loginForm = new FormGroup({
      email: this.email,
      password: this.password,
    });
  }
}
```
