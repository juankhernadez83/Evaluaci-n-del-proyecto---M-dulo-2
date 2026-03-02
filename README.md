# 📁 Angular + Bootstrap App — Proyecto Académico

Proyecto Angular que cumple los **10 requisitos** académicos solicitados.

---

## 🚀 Cómo ejecutar

```bash
# 1. Instalar dependencias (incluye Bootstrap)
npm install

# 2. Iniciar servidor de desarrollo
npm start

# 3. Abrir en el navegador
# http://localhost:4200
```

---

## ✅ Cumplimiento de Requisitos

### Requisito 1 — Bootstrap instalado como paquete npm
**Archivo:** `package.json`

```json
"dependencies": {
  "bootstrap": "^5.3.2"
}
```

Bootstrap está declarado como dependencia en `package.json` e instalado en `node_modules/` al ejecutar `npm install`.

---

### Requisito 2 — Importación de CSS de Bootstrap en estilos globales
**Archivo:** `src/styles.scss`

```scss
/* Importación de Bootstrap desde node_modules */
@import "bootstrap/dist/css/bootstrap.min.css";
```

La hoja de estilos global de Angular (`styles.scss`) importa Bootstrap, por lo que sus estilos están disponibles en toda la aplicación.

---

### Requisito 3 — Componente con estilos de Bootstrap
**Archivos:** `header.component.html`, `item-list.component.html`

Se usan clases Bootstrap como `navbar`, `navbar-dark`, `container`, `card`, `card-body`, `btn`, `badge`, `form-control`, `form-select`, `alert`, `list-unstyled`, entre muchas otras.

---

### Requisito 4 — Sintaxis `{{ }}` con variables TypeScript
**Archivos:** `header.component.ts` + `header.component.html`, `item-list.component.ts` + `item-list.component.html`

**TypeScript:**
```typescript
appNombre: string = 'Mi Gestor de Tareas';
appDescripcion: string = 'Organiza tus actividades de forma simple y elegante';
version: string = '1.0.0';
titulo: string = 'Lista de Tareas';
```

**HTML:**
```html
<span class="navbar-brand">{{ appNombre }}</span>
<small>{{ appDescripcion }}</small>
<span class="badge">v{{ version }}</span>
<h4>{{ titulo }}</h4>
<h2>{{ totalItems }}</h2>
<h6>{{ item.nombre }}</h6>
<p>{{ item.descripcion }}</p>
```

---

### Requisito 5 — Componente contenedor con array de objetos
**Archivo:** `src/app/components/item-list/item-list.component.ts`

```typescript
items: Item[] = [
  {
    id: 1,
    nombre: 'Estudiar Angular',
    descripcion: 'Repasar directivas, componentes y servicios',
    categoria: 'Estudio',
    fechaCreacion: new Date('2024-01-10')
  },
  // ... más elementos
];
```

El array `items` contiene objetos de tipo `Item` y se muestra en el navegador.

---

### Requisito 6 — Tag `<ul>` con `<li>` y directiva `*ngFor`
**Archivo:** `src/app/components/item-list/item-list.component.html`

```html
<ul class="list-unstyled">
  <li *ngFor="let item of items; let i = index" class="mb-3 item-animado">
    <div class="card border-0 shadow">
      <div class="card-body">
        <h6>{{ item.nombre }}</h6>
        <p>{{ item.descripcion }}</p>
        <span class="badge">{{ item.categoria }}</span>
      </div>
    </div>
  </li>
</ul>
```

La directiva `*ngFor` itera sobre el array `items` y genera un `<li>` por cada elemento.

---

### Requisito 7 — Uso de `@HostBinding`
**Archivos:** `header.component.ts`, `item-list.component.ts`

**Header:**
```typescript
import { Component, HostBinding } from '@angular/core';

@Component({ selector: 'app-header', ... })
export class HeaderComponent {
  @HostBinding('class') hostClass = 'w-100 d-block';
}
```

**ItemList:**
```typescript
@Component({ selector: 'app-item-list', ... })
export class ItemListComponent {
  @HostBinding('class') hostClass = 'container py-3 d-block';
}
```

`@HostBinding('class')` aplica clases CSS directamente al elemento host en el DOM, sin necesidad de un elemento wrapper extra en el HTML.

---

### Requisito 8 — Formulario HTML con variables de plantilla (`#`)
**Archivo:** `src/app/components/item-list/item-list.component.html`

```html
<form>
  <!-- #nombreInput es una variable de plantilla que referencia el input -->
  <input type="text" #nombreInput class="form-control" placeholder="Nombre..." />

  <!-- #descripcionInput referencia el textarea -->
  <textarea #descripcionInput class="form-control"></textarea>

  <!-- #categoriaInput referencia el select -->
  <select #categoriaInput class="form-select">
    <option value="Estudio">📚 Estudio</option>
    <option value="Desarrollo">💻 Desarrollo</option>
  </select>

  <button type="button" (click)="agregarItem(...)">
    ✚ Agregar Tarea
  </button>
</form>
```

Las variables `#nombreInput`, `#descripcionInput` y `#categoriaInput` son referencias locales al elemento DOM, que permiten acceder al valor del campo sin necesidad de `ngModel`.

---

### Requisito 9 — Click en submit invoca función TypeScript con variables `#`
**Archivo:** `src/app/components/item-list/item-list.component.html`

```html
<button
  type="button"
  class="btn w-100"
  (click)="agregarItem(
    nombreInput.value,
    descripcionInput.value,
    categoriaInput.value
  ); nombreInput.value = ''; descripcionInput.value = ''"
>
  ✚ Agregar Tarea
</button>
```

Al hacer clic, se invoca `agregarItem()` del componente TypeScript, pasando como argumentos los valores de las variables de plantilla `#nombreInput.value`, `#descripcionInput.value`, `#categoriaInput.value`.

---

### Requisito 10 — Función agrega al array y actualiza la UI reactivamente
**Archivo:** `src/app/components/item-list/item-list.component.ts`

```typescript
agregarItem(nombre: string, descripcion: string, categoria: string): void {
  if (!nombre.trim() || !descripcion.trim()) {
    return;  // Validación básica
  }

  const nuevoItem: Item = {
    id: this.proximoId++,
    nombre: nombre.trim(),
    descripcion: descripcion.trim(),
    categoria: categoria || 'General',
    fechaCreacion: new Date()
  };

  // Al hacer push(), Angular detecta el cambio y actualiza el *ngFor automáticamente
  this.items.push(nuevoItem);

  this.mensajeExito = `✅ "${nuevoItem.nombre}" fue agregado correctamente.`;
  setTimeout(() => { this.mensajeExito = ''; }, 3000);
}
```

Cuando se llama a `this.items.push(nuevoItem)`, Angular detecta el cambio en el array (gracias a Zone.js y Change Detection) y automáticamente re-renderiza el `*ngFor`, mostrando el nuevo ítem en la interfaz sin necesidad de recargar la página.

---

## 📂 Estructura del Proyecto

```
angular-bootstrap-app/
├── package.json                          ← Bootstrap como dependencia npm
├── angular.json                          ← Configuración Angular CLI
├── tsconfig.json
├── tsconfig.app.json
└── src/
    ├── index.html                        ← HTML raíz
    ├── main.ts                           ← Punto de entrada
    ├── styles.scss                       ← Estilos globales + @import Bootstrap
    └── app/
        ├── app.module.ts                 ← Módulo raíz con declaraciones
        ├── app.component.ts              ← Componente raíz
        ├── models/
        │   └── item.model.ts             ← Interfaz TypeScript Item
        └── components/
            ├── header/
            │   ├── header.component.ts   ← @HostBinding + variables {{ }}
            │   ├── header.component.html ← Bootstrap + interpolación
            │   └── header.component.scss
            └── item-list/
                ├── item-list.component.ts   ← Array, agregarItem(), @HostBinding
                ├── item-list.component.html ← *ngFor, formulario con #, {{ }}
                └── item-list.component.scss
```

---

## 🔑 Conceptos Angular Utilizados

| Concepto | Descripción | Requisito |
|---|---|---|
| `{{ }}` | Interpolación de datos | #4 |
| `*ngFor` | Directiva estructural para iterar listas | #6 |
| `*ngIf` | Directiva estructural condicional | — |
| `(click)` | Event binding | #9 |
| `#variable` | Variables de plantilla local | #8, #9 |
| `@HostBinding` | Bind de propiedad al elemento host | #7 |
| `@Component` | Decorador de componente | — |
| `pipe date` | Pipe de transformación de fecha | — |
