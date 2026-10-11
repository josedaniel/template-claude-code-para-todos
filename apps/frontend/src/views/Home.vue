<script setup lang="ts">
import { ref, onMounted } from "vue";
import {
  Database,
  FileText,
  FolderOpen,
  Leaf,
  Moon,
  Palette,
  Server,
  Sparkles,
  Sun,
  Users,
} from "lucide-vue-next";

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

// Forma de hoja reutilizada en las ramas decorativas
const hoja = "M0 0C8-16 34-16 46 0 34 16 8 16 0 0ZM2 0H40";
const rama = [
  [45, 320, -35, 1.2],
  [45, 320, -150, 1],
  [70, 255, -30, 1.3],
  [70, 255, -155, 1.1],
  [100, 190, -25, 1.2],
  [100, 190, -160, 1],
  [135, 120, -30, 1],
  [135, 120, -150, 0.85],
  [170, 40, -80, 0.9],
];

const incluye = [
  {
    icono: Database,
    titulo: "Base de datos",
    texto: "SQLite guarda tu información en un solo archivo de tu computadora.",
  },
  {
    icono: Server,
    titulo: "Servidor local",
    texto: "Procesa tus archivos sin enviarlos a internet.",
  },
  {
    icono: Users,
    titulo: "Usuarios",
    texto: "Registro e inicio de sesión ya configurados, con una cuenta por persona.",
  },
  {
    icono: FolderOpen,
    titulo: "Carpeta inbox/",
    texto: "El lugar para dejar tus archivos de prueba: hojas de Excel, PDF o imágenes.",
  },
  {
    icono: FileText,
    titulo: "Instrucciones para Claude",
    texto: "Un archivo AGENTS.md que mantiene el proyecto ordenado y predecible.",
  },
  {
    icono: Palette,
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
        <span
          class="flex h-8 w-8 items-center justify-center rounded-[10px] bg-primary text-on-primary"
          aria-hidden="true"
        >
          <Leaf :size="18" :stroke-width="2.2" />
        </span>
        <span class="font-serif font-semibold text-[18px] tracking-[-0.3px]"
          >Claude Code para Todos</span
        >
      </a>
      <div class="flex items-center gap-5">
        <a
          href="#incluye"
          class="text-sm font-medium text-muted no-underline transition-colors hover:text-accent max-sm:hidden"
          >Qué incluye</a
        >
        <a
          href="#empezar"
          class="text-sm font-medium text-muted no-underline transition-colors hover:text-accent max-sm:hidden"
          >Cómo empezar</a
        >
        <button
          class="bg-transparent border border-line rounded-full w-9 h-9 cursor-pointer p-0 flex items-center justify-center text-muted transition-colors hover:text-accent hover:border-accent"
          :aria-label="theme === 'dark' ? 'Cambiar a modo claro' : 'Cambiar a modo oscuro'"
          @click="toggleTheme"
        >
          <Moon v-if="theme === 'dark'" :size="16" aria-hidden="true" />
          <Sun v-else :size="16" aria-hidden="true" />
        </button>
      </div>
    </nav>

    <main class="flex-1">
      <!-- Portada -->
      <section class="relative overflow-hidden">
        <!-- Ramas decorativas -->
        <svg
          class="absolute -right-10 top-0 h-[460px] w-[280px] text-accent opacity-[0.14] max-md:hidden"
          viewBox="0 0 240 400"
          fill="none"
          aria-hidden="true"
        >
          <path
            d="M20 380C60 280 90 180 170 40"
            stroke="currentColor"
            stroke-width="3"
            stroke-linecap="round"
          />
          <path
            v-for="([x, y, giro, escala], i) in rama"
            :key="i"
            :d="hoja"
            :transform="`translate(${x} ${y}) rotate(${giro}) scale(${escala})`"
            fill="currentColor"
            stroke="var(--color-bg)"
            stroke-width="1.2"
          />
        </svg>
        <svg
          class="absolute -left-16 bottom-0 h-[340px] w-[210px] text-accent opacity-[0.1] -scale-x-100 max-md:hidden"
          viewBox="0 0 240 400"
          fill="none"
          aria-hidden="true"
        >
          <path
            d="M20 380C60 280 90 180 170 40"
            stroke="currentColor"
            stroke-width="3"
            stroke-linecap="round"
          />
          <path
            v-for="([x, y, giro, escala], i) in rama"
            :key="i"
            :d="hoja"
            :transform="`translate(${x} ${y}) rotate(${giro}) scale(${escala})`"
            fill="currentColor"
            stroke="var(--color-bg)"
            stroke-width="1.2"
          />
        </svg>
        <div
          class="absolute left-1/2 top-[-180px] h-[420px] w-[720px] -translate-x-1/2 rounded-full bg-accent-soft blur-3xl opacity-70"
          aria-hidden="true"
        ></div>

        <div
          class="relative max-w-[1080px] mx-auto px-8 pt-24 pb-24 max-sm:px-4 max-sm:pt-12 max-sm:pb-14"
        >
          <div
            class="grid grid-cols-[1.1fr_1fr] gap-14 items-center max-md:grid-cols-1 max-md:gap-10"
          >
            <div>
              <p
                class="inline-flex items-center gap-2 text-[12px] font-bold tracking-[1.2px] uppercase text-accent bg-accent-soft rounded-full py-1.5 px-3.5 mb-6"
              >
                <Sparkles :size="14" aria-hidden="true" />
                Plantilla de aplicación web
              </p>
              <h1
                class="font-serif text-[clamp(38px,5vw,56px)] font-semibold tracking-[-1.2px] leading-[1.05] text-ink mb-6"
              >
                Construye tu propia herramienta, sin ser programador
              </h1>
              <p class="text-lg text-muted leading-[1.7] max-w-[470px] mb-9">
                La base lista sobre la que Claude Code construye lo que necesitas. Tú pones las
                reglas; el código se escribe solo.
              </p>
              <div class="flex gap-3 flex-wrap">
                <a
                  href="#empezar"
                  class="bg-primary text-on-primary text-[15px] font-semibold py-[13px] px-7 rounded-full no-underline shadow-[0_8px_20px_-8px_var(--color-primary)] transition-colors hover:bg-primary-hover"
                  >Cómo empezar</a
                >
                <a
                  href="#incluye"
                  class="text-ink text-[15px] font-semibold py-[13px] px-7 rounded-full no-underline border border-line transition-colors hover:border-accent hover:text-accent"
                  >Qué incluye</a
                >
              </div>
            </div>

            <!-- Conversación de ejemplo -->
            <div
              class="border border-line rounded-[28px] bg-subtle p-5 grid gap-3.5 shadow-[0_28px_56px_-28px_rgba(31,26,10,0.25)]"
              aria-label="Ejemplo de conversación con Claude"
            >
              <div class="flex items-center gap-2 pb-1 text-muted" aria-hidden="true">
                <span
                  class="w-6 h-6 rounded-full bg-accent-soft text-accent flex items-center justify-center"
                >
                  <Sparkles :size="13" :stroke-width="2.2" />
                </span>
                <span class="text-xs font-semibold tracking-wide">Claude Code</span>
              </div>
              <div
                class="justify-self-end max-w-[88%] bg-primary text-on-primary text-sm leading-normal py-3 px-4 rounded-[20px] rounded-br-[6px]"
              >
                Lee inbox/ventas.xlsx y crea una página con el total vendido por mes, ordenado de
                mayor a menor.
              </div>
              <div
                class="justify-self-start max-w-[88%] bg-bg border border-line text-body text-sm leading-normal py-3 px-4 rounded-[20px] rounded-bl-[6px]"
              >
                Listo. Creé la página <strong class="text-ink">Ventas por mes</strong> y la conecté
                a la base de datos. Ábrela en <strong class="text-ink">localhost:5173</strong> y
                dime qué ajustar.
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Qué incluye -->
      <section id="incluye" class="bg-subtle border-y border-line scroll-mt-16">
        <div class="max-w-[1080px] mx-auto px-8 py-20 max-sm:px-4 max-sm:py-14">
          <p class="text-[12px] font-bold tracking-[1.2px] uppercase text-accent mb-3">Incluido</p>
          <h2 class="font-serif text-[34px] font-semibold tracking-[-0.7px] leading-[1.1] mb-3">
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
              class="group bg-bg border border-line rounded-[22px] p-6 transition-all duration-200 hover:-translate-y-0.5 hover:border-accent hover:shadow-[0_18px_32px_-22px_rgba(31,26,10,0.3)]"
            >
              <span
                class="mb-4 flex h-11 w-11 items-center justify-center rounded-[14px_14px_14px_4px] bg-accent-soft text-accent"
              >
                <component :is="item.icono" :size="22" :stroke-width="1.8" aria-hidden="true" />
              </span>
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
        <p class="text-[12px] font-bold tracking-[1.2px] uppercase text-accent mb-3">
          Primeros pasos
        </p>
        <h2 class="font-serif text-[34px] font-semibold tracking-[-0.7px] leading-[1.1] mb-3">
          Tres pasos, ningún comando
        </h2>
        <p class="text-base text-muted leading-[1.6] mb-10 max-w-[520px]">
          Tu rol es el de quien dirige: defines las reglas y revisas el resultado. Claude escribe el
          código.
        </p>
        <ol class="grid grid-cols-3 gap-6 list-none max-md:grid-cols-1">
          <li v-for="(paso, i) in pasos" :key="paso.titulo" class="relative pt-5">
            <span class="absolute left-0 top-0 h-[3px] w-full rounded-full bg-line"></span>
            <span class="absolute left-0 top-0 h-[3px] w-12 rounded-full bg-accent"></span>
            <span
              class="block font-serif text-[48px] font-semibold leading-none text-accent mb-3"
              >{{ i + 1 }}</span
            >
            <p class="font-bold text-[16px] leading-[1.3] mb-2">{{ paso.titulo }}</p>
            <p class="text-sm text-muted leading-[1.6]">{{ paso.texto }}</p>
          </li>
        </ol>

        <div class="mt-12 border border-line rounded-[28px] p-7 bg-subtle max-sm:p-5">
          <h3 class="font-serif text-xl font-semibold mb-1.5">Un primer pedido de ejemplo</h3>
          <p class="text-sm text-muted mb-4">
            Cópialo en Claude Code y ajústalo a lo que necesitas.
          </p>
          <pre
            class="font-mono text-[13px] leading-[1.6] text-body bg-bg border border-line border-l-[3px] border-l-accent rounded-xl p-4 whitespace-pre-wrap"
          >
Lee el archivo inbox/ventas.xlsx y crea una página que muestre el total vendido por mes, ordenado de mayor a menor. Usa únicamente los datos del archivo.</pre>
        </div>
      </section>
    </main>

    <!-- Pie de página -->
    <footer class="bg-footer text-[#d9d1b5]">
      <div
        class="max-w-[1080px] mx-auto px-8 py-10 flex items-center justify-between gap-4 max-sm:px-4 max-sm:flex-col max-sm:text-center"
      >
        <span class="flex items-center gap-2.5 font-serif font-semibold text-[#fbf6e3]">
          <span
            class="flex h-6 w-6 items-center justify-center rounded-lg bg-primary text-on-primary"
            aria-hidden="true"
          >
            <Leaf :size="14" :stroke-width="2.2" />
          </span>
          Claude Code para Todos
        </span>
        <span class="text-[13px] text-[#a49b7c]">© {{ year }} Claude Code para Todos</span>
      </div>
    </footer>
  </div>
</template>
