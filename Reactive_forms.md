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

## Formulario HTML

5. ahora creamos el formulario html y lo vinculamos al ts

```html
<form [formGroup]="loginForm"></form>
```

- Ya tenemos el formulario vinculado

6. Ahora lo que tenemos que vincular es:

- Cada input con el campo del formulario reactivo

```html
<input type="text" id="login-email" formControlName="email" />
```

- `formControlName` vincula el input con el campo del formulario reactivo.
- Y añadimos los `label`.

```html
<fieldset>
  <label for="login-email">Login Email:</label>
  <input type="text" id="login-email" formControlName="email" />
</fieldset>
<fieldset>
  <label for="login-password">Login Password:</label>
  <input type="password" id="login-password" formControlName="password" />
</fieldset>
```

> `fieldset` es una etiqueta HTML que se utiliza para agrupar elementos relacionados dentro de un formulario.Su propósito principal es mejorar la accesibilidad y organización visual de los formularios

7. Para probarlo vamos a crear un boton y recoger el `evento submit` y llamamos dentro al `metodo handlesubmit`.

```html
<form [formGroup]="loginForm" (submit)="handlesubmit()">
  <fieldset>
    <label for="login-email">Login Email:</label>
    <input type="text" id="login-email" formControlName="email" />
  </fieldset>
  <fieldset>
    <label for="login-password">Login Password:</label>
    <input type="password" id="login-password" formControlName="password" />
  </fieldset>
  <button>Login</button>
</form>
```

- handlesubmit es un metodo que no va a recibir ni devolver nada.
- lo unico que hace es un colsole.log con el valor del formulario reactivo.

```ts
handlesubmit(): void {
    console.log(this.loginForm.value);
  }
```

```html
<form
  [formGroup]="loginForm"
  (submit)="handlesubmit()"
  class="max-w-md mx-auto mt-10 p-6 bg-white rounded-xl shadow-md space-y-6"
>
  <fieldset class="flex flex-col space-y-2">
    <label for="login-email" class="text-sm font-medium text-gray-700"
      >Email:</label
    >
    <input
      type="text"
      id="login-email"
      formControlName="email"
      class="px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
    />
  </fieldset>
  <fieldset class="flex flex-col space-y-2">
    <label for="login-password" class="text-sm font-medium text-gray-700"
      >Password:</label
    >
    <input
      type="password"
      id="login-password"
      formControlName="password"
      class="px-4 py-2 border border-gray-300 rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500"
    />
  </fieldset>
  <button
    class="w-full py-2 px-4 bg-blue-600 text-white font-semibold rounded-md hover:bg-blue-700 transition-colors duration-200"
  >
    Login
  </button>
</form>
```

## Validators

8. en el constructor damos un segundo argumento. los validators.

- Validators.required para que el campo sea requerido
- Desavilitamos el boton si no se rellenan los campos requeridos.

```html
<button
  [disabled]="loginForm.invalid"
  class="w-full py-2 px-4 bg-blue-600 text-white font-semibold rounded-md hover:bg-blue-700 transition-colors duration-200"
>
  Login
</button>
```

9. Debera ser un array el validator si queremos añadir mas parametros.

✅ Para el email:

```ts
this.email = new FormControl("", [
  Validators.required,
  Validators.email, // Verifica que tenga un formato válido de email
]);
```

Puedes usar también un patrón personalizado si necesitas más control, como:

```ts
Validators.pattern(/^[^\s@]+@[^\s@]+\.[^\s@]{2,}$/);
```

✅ Para la contraseña:

```ts
this.password = new FormControl("", [
  Validators.required,
  Validators.minLength(6), // Mínimo 6 caracteres (o el que desees)
  Validators.maxLength(32), // Máximo 32 caracteres (opcional)
  Validators.pattern(/^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d@$!%*?&]+$/),
  // Al menos una letra y un número
]);
```

💡 Puedes ajustar el patrón según tus requisitos de seguridad. Ejemplo para exigir:

Mínimo 1 mayúscula

1 minúscula

1 número

1 carácter especial:

```ts
Validators.pattern(
  /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/
);
```
