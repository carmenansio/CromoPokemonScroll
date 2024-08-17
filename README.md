Vamos a hacer algo súper chulo: un cromo animado en 3D que gira mientras haces scroll. ¿Lo mejor? No necesitamos ni una sola línea de `JavaScript`. Vamos a hacerlo todo con `HTML` y `CSS`. Te prometo que es más fácil de lo que parece, ¡así que vamos allá!

## Paso 1: La Estructura básica en `HTML`

Antes de meternos de lleno en la animación, necesitamos construir la estructura básica de nuestro cromo. Esto lo hacemos en `HTML`.

```html
<div class="container">
	<div class="card">
		<div class="card__front">
			<img src="https://assets.codepen.io/527512/card_pikachu.jpg" />
		</div>
		<div class="card__front card__front--back">
			<img src="https://assets.codepen.io/527512/card_pokemon.jpg" />
		</div>
	</div>
</div>
```

### ¿Qué está pasando aquí?

- `<div class="container">`: Este `div` es nuestro contenedor principal. Es como una caja que contiene nuestra tarjeta. Lo necesitamos para poder centrar la tarjeta y aplicarle el efecto 3D.

- `<div class="card">`: Aquí tenemos la tarjeta en sí. Piensa en ella como un libro con dos páginas: una delantera y una trasera.

- `<div class="card__front">`: Esta es la parte frontal de la tarjeta, donde colocamos la primera imagen (en este caso, una de Pikachu).

- `<div class="card__front card__front--back">`: Y esta es la parte trasera, con otra imagen (el clásico diseño de carta Pokémon). Notarás que le hemos añadido una clase extra (`card__front--back`), que luego usaremos en el `CSS` para que esta cara esté "detrás" de la tarjeta cuando gire.

## Paso 2: Agregando estilo con `CSS`

Ahora que tenemos la estructura básica, vamos a darle vida con `CSS`. Aquí es donde empieza la magia. Vamos a dividirlo en secciones para que sea más fácil de seguir.

```css
body {
	min-block-size: 160dvh;
	display: grid;
	place-content: center;
	background: black;
}
```

### Detalle del código CSS

- `<body>`: Configuramos el cuerpo del documento para que todo esté centrado en la pantalla y le damos un fondo negro. La propiedad `min-block-size: 160dvh` asegura que el contenido ocupe más de la altura completa de la pantalla, lo que es útil para habilitar el scroll.

```css
.container {
	--width: 240px;
	--height: 330px;
	width: 100vw;
	display: flex;
	justify-content: center;
	perspective: 300px;
```

- `<container>`: Aquí es donde las cosas se ponen interesantes. Estamos definiendo las dimensiones de nuestra tarjeta usando variables CSS (`--width` y `--height`). Además, estamos usando `perspective: 300px`, que es lo que le da ese efecto 3D. Esta propiedad crea la sensación de profundidad, como si el cromo estuviera flotando en el espacio.

```css
	.card {
		position: relative;
		width: var(--width);
		height: var(--height);
		cursor: pointer;
		transition: 1.1s ease-in-out;
		transform-style: preserve-3d;

		/* Aquí se define la animación de la tarjeta */
		animation: rotate ease-in-out;
		animation-timeline: scroll(); /* Asociando la animación al scroll */
	}
```

- `.card`: Este es el estilo del cromo. Le damos un tamaño usando las variables definidas antes. La propiedad `transform-style: preserve-3d` es crucial porque asegura que las caras de la tarjeta se comporten como partes de un objeto 3D.

- `animation: rotate ease-in-out`: Aquí es donde le decimos al cromo que gire. Esta animación se ejecutará con una transición suave (`ease-in-out`) cada vez que haya scroll.

- `animation-timeline: scroll()`: Este es el truco del scroll. Con esta propiedad, vinculamos la animación al desplazamiento de la página. Así, el cromo girará a medida que bajes o subas.

```css
.card__front {
	position: absolute;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
	backface-visibility: hidden;
	-webkit-box-reflect: below -12px linear-gradient(transparent, #00000000, rgb(0
					0 0 / 14%));

	img {
		width: var(--width);
		height: var(--height);
		object-fit: cover;
		border-radius: 1rem;
	}

	&--back {
		transform: rotateY(0.5turn);
	}
}
```

### Detalle del estilo de las caras del cromo

- `.card__front`**: Le damos a cada cara de la tarjeta una posición absoluta para que se superpongan correctamente. `backface-visibility: hidden` asegura que la cara que no está mirando hacia nosotros no sea visible (evitando ese feo parpadeo cuando gira).

- `.card__front img`: Estilizamos las imágenes para que se ajusten al tamaño del cromo y se vean bien, aplicando un borde redondeado (`border-radius: 1rem`) para un toque más parecido al original.

- `&--back`: Aquí viene la parte clave para la cara trasera. Usamos `transform: rotateY(0.5turn)` para girarla 180 grados (media vuelta) en el eje Y, colocándola detrás del front del cromo. Esto es lo que hará que parezca que el cromo tiene dos lados.

```css
@keyframes rotate {
	to {
		transform: rotateY(1turn); /* Gira la tarjeta 360 grados */
	}
}
```

- `@keyframes rotate`: Este es el motor de nuestra animación. Básicamente, estamos diciendo: "oye, cromo, cuando te animes, gira 360 grados en el eje Y". Al hacer scroll, esta rotación se va a activar, gracias a la propiedad `animation-timeline` que vinculamos antes.

{% codepen https://codepen.io/carmenansio/pen/jOjMZRo %}

## ¿Qué son las animaciones basadas en Scroll (Scroll-Driven Animations)?

Las **scroll-driven animations** (animaciones basadas en scroll) son una técnica en la que el desplazamiento (scroll) de la página se utiliza como disparador para iniciar, controlar o influir en una animación. Tradicionalmente, esto se hacía con `JavaScript`, manipulando el `DOM` para detectar el desplazamiento y ajustar propiedades `CSS` en consecuencia.

Pero ahora, gracias a las nuevas capacidades de `CSS`, ¡podemos hacer todo esto sin una sola línea de `JavaScript`! Con propiedades como `scroll-timeline` y `animation-timeline`, podemos vincular el progreso de una animación directamente al scroll, creando efectos súper suaves y bien sincronizados que antes solo eran posibles con `JavaScript`.

### ¿Por qué deberías usarlas?

- **Rendimiento**: Las animaciones en `CSS` suelen ser más rápidas y suaves porque están optimizadas por el navegador. No tienes que preocuparte por la sobrecarga de scripts.

- **Facilidad de mantenimiento**: Es mucho más sencillo mantener y ajustar tu código cuando todo está en una hoja de estilos. No hay funciones complejas de `JavaScript` a las que hacer seguimiento.

- **Interactividad mejorada**: Añadir animaciones que respondan al desplazamiento de la página mejora la experiencia del usuario, haciendo que tu sitio se sienta más dinámico y vivo.

## Paso 3: Experimenta y personaliza

Ya tienes la base, pero esto es solo el comienzo. Puedes jugar con los valores de `perspective`, `width`, `height`, y la duración de la animación (`transition: 1.1s`). ¿Qué pasa si cambias el tiempo? ¿O si usas otras imágenes? ¡Las posibilidades son infinitas!

Por ejemplo, podrías cambiar `perspective` a un valor mayor o menor y ver cómo eso afecta la profundidad 3D del cromo. También puedes ajustar la velocidad de la rotación para hacerla más rápida o más lenta, dependiendo de lo que mejor se ajuste a tu proyecto.

## Conclusión

Y ahí lo tienes, un cromo animado en 3D que gira mientras haces scroll, ¡y todo solo con `HTML` y `CSS`! Espero que hayas disfrutado este tutorial tanto como yo disfruté creándolo. Lo más importante aquí es experimentar y divertirse. No tengas miedo de romper las cosas; es así como se aprende.

¡Comparte lo que hagas! Estoy deseando ver qué increíbles cards y efectos 3D crearás con estas técnicas.