<script setup lang="ts">
import { ref, onMounted } from "vue";

const theme = ref("light");
const year = new Date().getFullYear();

const toggleTheme = () => {
  const nextTheme = theme.value === "dark" ? "light" : "dark";
  theme.value = nextTheme;
  document.documentElement.setAttribute("data-theme", nextTheme);
  localStorage.setItem("theme", nextTheme);
};

onMounted(() => {
  const saved = localStorage.getItem("theme");
  const prefersDark = window.matchMedia("(prefers-color-scheme: dark)").matches;
  theme.value = saved || (prefersDark ? "dark" : "light");
  document.documentElement.setAttribute("data-theme", theme.value);
});

const incluye = [
  {
    titulo: "Base de datos",
    texto: "SQLite guarda tu información en un solo archivo de tu computadora.",
  },
  {
    titulo: "Servidor local",
    texto: "Procesa tus archivos sin enviarlos a internet.",
  },
  {
    titulo: "Usuarios",
    texto: "Registro e inicio de sesión ya configurados, con una cuenta por persona.",
  },
  {
    titulo: "Carpeta inbox/",
    texto: "El lugar para dejar tus archivos de prueba: hojas de Excel, PDF o imágenes.",
  },
  {
    titulo: "Instrucciones para Claude",
    texto: "Un archivo AGENTS.md que mantiene el proyecto ordenado y predecible.",
  },
  {
    titulo: "Design system",
    texto: "Botones y colores reutilizables para que todo se vea coherente.",
  },
];

const pasos = [
  {
    titulo: "Abre esta carpeta en Claude Code",
    texto:
      "Selecciona la carpeta del proyecto como espacio de trabajo. Claude lee las instrucciones del proyecto antes de tocar cualquier archivo.",
  },
  {
    titulo: "Deja tus archivos en inbox/",
    texto: "Hojas de Excel, documentos o imágenes. Usa siempre datos ficticios o anonimizados.",
  },
  {
    titulo: "Pídele lo que necesitas",
    texto:
      "Descríbelo como se lo explicarías a un compañero nuevo: qué entra, qué reglas aplica y qué debe salir.",
  },
];
</script>

<template>
  <div class="flex flex-col min-h-screen">
    <!-- Navegación -->
    <nav
      class="sticky top-0 z-20 flex items-center justify-between h-16 px-8 bg-nav-bg backdrop-blur-md border-b border-line max-sm:px-4"
    >
      <a href="/" class="flex items-center gap-2.5 no-underline text-ink" aria-label="Inicio">
        <img src="/favicon.svg" alt="" class="w-7 h-7 block" />
        <span class="font-serif font-bold text-[17px] tracking-[-0.2px]"
          >Claude Code para Todos</span
        >
      </a>
      <div class="flex items-center gap-5">
        <a
          href="#empezar"
          class="text-sm font-medium text-muted no-underline transition-colors hover:text-ink max-sm:hidden"
          >Cómo empezar</a
        >
        <button
          class="bg-transparent border border-line rounded-full w-9 h-9 cursor-pointer p-0 flex items-center justify-center text-muted transition-colors hover:text-ink hover:border-ink"
          :aria-label="theme === 'dark' ? 'Cambiar a modo claro' : 'Cambiar a modo oscuro'"
          @click="toggleTheme"
        >
          <!-- Luna — modo oscuro -->
          <svg
            v-if="theme === 'dark'"
            width="16"
            height="16"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            aria-hidden="true"
          >
            <path d="M12 3a6 6 0 0 0 9 9 9 9 0 1 1-9-9Z" />
          </svg>
          <!-- Sol — modo claro -->
          <svg
            v-else
            width="16"
            height="16"
            viewBox="0 0 24 24"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
            aria-hidden="true"
          >
            <circle cx="12" cy="12" r="4" />
            <path
              d="M12 2v2M12 20v2M4.93 4.93l1.41 1.41M17.66 17.66l1.41 1.41M2 12h2M20 12h2M6.34 17.66l-1.41 1.41M19.07 4.93l-1.41 1.41"
            />
          </svg>
        </button>
      </div>
    </nav>

    <main class="flex-1">
      <!-- Portada -->
      <section
        class="max-w-[1080px] mx-auto px-8 pt-24 pb-20 max-sm:px-4 max-sm:pt-12 max-sm:pb-14"
      >
        <div
          class="grid grid-cols-[1.1fr_1fr] gap-14 items-center max-md:grid-cols-1 max-md:gap-10"
        >
          <div>
            <p
              class="inline-block text-[12px] font-bold tracking-[1px] uppercase text-accent bg-accent-soft rounded-full py-1.5 px-3.5 mb-6"
            >
              Plantilla de aplicación web
            </p>
            <h1
              class="font-serif text-[clamp(36px,4.8vw,52px)] font-bold tracking-[-1px] leading-[1.05] text-ink mb-6"
            >
              Construye tu propia herramienta, sin ser programador
            </h1>
            <p class="text-lg text-muted leading-[1.7] max-w-[460px] mb-9">
              La base lista sobre la que Claude Code construye lo que necesitas. Tú pones las
              reglas; el código se escribe solo.
            </p>
            <div class="flex gap-3 flex-wrap">
              <a
                href="#empezar"
                class="bg-primary text-white text-[15px] font-semibold py-[13px] px-7 rounded-full no-underline transition-colors hover:bg-primary-hover"
                >Cómo empezar</a
              >
              <a
                href="#incluye"
                class="text-ink text-[15px] font-semibold py-[13px] px-7 rounded-full no-underline border border-line transition-colors hover:border-ink"
                >Qué incluye</a
              >
            </div>
          </div>

          <!-- Conversación de ejemplo -->
          <div
            class="border border-line rounded-[20px] bg-subtle p-5 grid gap-3.5 shadow-[0_24px_48px_-24px_rgba(28,25,23,0.25)]"
            aria-label="Ejemplo de conversación con Claude"
          >
            <div class="flex items-center gap-1.5 pb-1" aria-hidden="true">
              <span class="w-2.5 h-2.5 rounded-full bg-line"></span>
              <span class="w-2.5 h-2.5 rounded-full bg-line"></span>
              <span class="w-2.5 h-2.5 rounded-full bg-line"></span>
            </div>
            <div
              class="justify-self-end max-w-[88%] bg-primary text-white text-sm leading-normal py-3 px-4 rounded-[16px] rounded-br-[4px]"
            >
              Lee inbox/ventas.xlsx y crea una página con el total vendido por mes, ordenado de
              mayor a menor.
            </div>
            <div
              class="justify-self-start max-w-[88%] bg-bg border border-line text-body text-sm leading-normal py-3 px-4 rounded-[16px] rounded-bl-[4px]"
            >
              Listo. Creé la página <strong class="text-ink">Ventas por mes</strong> y la conecté a
              la base de datos. Ábrela en <strong class="text-ink">localhost:5173</strong> y dime
              qué ajustar.
            </div>
          </div>
        </div>
      </section>

      <!-- Qué incluye -->
      <section id="incluye" class="bg-subtle border-y border-line scroll-mt-16">
        <div class="max-w-[1080px] mx-auto px-8 py-20 max-sm:px-4 max-sm:py-14">
          <h2 class="font-serif text-[32px] font-bold tracking-[-0.5px] mb-3">
            No arrancas desde una carpeta vacía
          </h2>
          <p class="text-base text-muted leading-[1.6] mb-10 max-w-[520px]">
            La parte técnica ya está resuelta y probada. Tú te enfocas en lo que hace única tu
            herramienta.
          </p>
          <div class="grid grid-cols-3 gap-4 max-md:grid-cols-2 max-sm:grid-cols-1">
            <div
              v-for="item in incluye"
              :key="item.titulo"
              class="bg-bg border border-line rounded-[16px] p-6"
            >
              <p class="font-bold text-[16px] leading-[1.3] mb-2 text-ink">{{ item.titulo }}</p>
              <p class="text-sm text-muted leading-[1.6]">{{ item.texto }}</p>
            </div>
          </div>
        </div>
      </section>

      <!-- Cómo empezar -->
      <section
        id="empezar"
        class="max-w-[1080px] mx-auto px-8 py-20 scroll-mt-16 max-sm:px-4 max-sm:py-14"
      >
        <h2 class="font-serif text-[32px] font-bold tracking-[-0.5px] mb-3">
          Tres pasos, ningún comando
        </h2>
        <p class="text-base text-muted leading-[1.6] mb-10 max-w-[520px]">
          Tu rol es el de quien dirige: defines las reglas y revisas el resultado. Claude escribe el
          código.
        </p>
        <ol class="grid grid-cols-3 gap-4 list-none max-md:grid-cols-1">
          <li v-for="(paso, i) in pasos" :key="paso.titulo" class="border-t-2 border-ink pt-5">
            <span class="block font-serif text-[44px] font-bold leading-none text-accent mb-3">{{
              i + 1
            }}</span>
            <p class="font-bold text-[16px] leading-[1.3] mb-2">{{ paso.titulo }}</p>
            <p class="text-sm text-muted leading-[1.6]">{{ paso.texto }}</p>
          </li>
        </ol>

        <div class="mt-12 border border-line rounded-[20px] p-7 bg-subtle max-sm:p-5">
          <h3 class="font-serif text-xl font-bold mb-1.5">Un primer pedido de ejemplo</h3>
          <p class="text-sm text-muted mb-4">
            Cópialo en Claude Code y ajústalo a lo que necesitas.
          </p>
          <pre
            class="font-mono text-[13px] leading-[1.6] text-body bg-bg border border-line rounded-xl p-4 whitespace-pre-wrap"
          >
Lee el archivo inbox/ventas.xlsx y crea una página que muestre el total vendido por mes, ordenado de mayor a menor. Usa únicamente los datos del archivo.</pre>
        </div>
      </section>
    </main>

    <!-- Pie de página -->
    <footer class="border-t border-line py-7 px-6 text-center text-[13px] text-muted">
      © {{ year }} Claude Code para Todos
    </footer>
  </div>
</template>
