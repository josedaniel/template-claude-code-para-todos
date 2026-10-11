<script setup lang="ts">
import { ref, onMounted } from "vue";
import {
  AppWindow,
  Check,
  Component,
  Copy,
  Database,
  FileText,
  Inbox,
  KeyRound,
  Moon,
  Paintbrush,
  Server,
  Sun,
  Terminal,
  Zap,
} from "lucide-vue-next";

const theme = ref("light");
const copiado = ref<number | null>(null);
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

const copiar = async (texto: string, indice: number) => {
  await navigator.clipboard.writeText(texto);
  copiado.value = indice;
  setTimeout(() => {
    if (copiado.value === indice) copiado.value = null;
  }, 1500);
};

const prompts = [
  {
    titulo: "Leer un archivo de Excel",
    texto:
      "Lee el archivo inbox/ventas.xlsx y crea una página que muestre el total vendido por mes, ordenado de mayor a menor. Usa únicamente los datos del archivo.",
  },
  {
    titulo: "Guardar información",
    texto:
      "Crea una página con un formulario para registrar clientes (nombre, correo y teléfono) y una tabla que los liste. Guarda los datos en la base de datos.",
  },
  {
    titulo: "Proteger una página",
    texto:
      "Haz que la página de clientes solo la vean las personas que iniciaron sesión, y muestra su nombre en la barra superior.",
  },
  {
    titulo: "Importar datos",
    texto:
      "Importa inbox/productos.csv a la base de datos y crea una página para buscar los productos por nombre.",
  },
];

const stack = [
  {
    icono: AppWindow,
    nombre: "Vue 3 + TypeScript",
    texto: "Construye lo que ves en pantalla: páginas, formularios y tablas.",
  },
  {
    icono: Paintbrush,
    nombre: "Tailwind CSS",
    texto: "Da el estilo a la interfaz: colores, espacios y tipografía.",
  },
  {
    icono: Component,
    nombre: "Design system",
    texto: "Componentes reutilizables, como los botones, para que todo se vea igual de coherente.",
  },
  {
    icono: Server,
    nombre: "Hono",
    texto:
      "El servidor. Recibe lo que pide la interfaz, procesa tus archivos y habla con la base de datos.",
  },
  {
    icono: Database,
    nombre: "SQLite + Drizzle",
    texto:
      "SQLite guarda tu información en un solo archivo de tu computadora. Drizzle define las tablas y hace las consultas.",
  },
  {
    icono: KeyRound,
    nombre: "Better Auth",
    texto: "Registro e inicio de sesión, con una cuenta por persona.",
  },
  {
    icono: Zap,
    nombre: "Vite+ (vp)",
    texto: "Instala, arranca y revisa el proyecto con comandos como vp run dev y vp check.",
  },
  {
    icono: Inbox,
    nombre: "Carpeta inbox/",
    texto: "El lugar para dejar tus archivos de prueba: hojas de Excel, PDF o imágenes.",
  },
  {
    icono: FileText,
    nombre: "AGENTS.md",
    texto: "Las instrucciones que Claude lee antes de tocar el proyecto, para mantenerlo ordenado.",
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
          class="flex h-8 w-8 items-center justify-center rounded-lg bg-primary text-on-primary"
          aria-hidden="true"
        >
          <Terminal :size="17" :stroke-width="2.2" />
        </span>
        <span class="font-serif font-semibold text-[18px] tracking-[-0.3px]"
          >Claude Code para Todos</span
        >
      </a>
      <div class="flex items-center gap-5">
        <a
          href="#prompts"
          class="text-sm font-medium text-muted no-underline transition-colors hover:text-ink max-sm:hidden"
          >Prompts</a
        >
        <a
          href="#stack"
          class="text-sm font-medium text-muted no-underline transition-colors hover:text-ink max-sm:hidden"
          >Stack</a
        >
        <button
          class="bg-transparent border border-line rounded-full w-9 h-9 cursor-pointer p-0 flex items-center justify-center text-muted transition-colors hover:text-ink hover:border-ink"
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
      <section class="max-w-[960px] mx-auto px-8 pt-24 pb-16 max-sm:px-4 max-sm:pt-14 max-sm:pb-10">
        <h1
          class="font-serif text-[clamp(36px,5vw,54px)] font-semibold tracking-[-1.2px] leading-[1.08] text-ink mb-5 max-w-[640px]"
        >
          Construye tu propia herramienta, sin ser programador
        </h1>
        <p class="text-lg text-muted leading-[1.7] max-w-[560px] mb-8">
          Una base lista con servidor, base de datos y usuarios. Abre la carpeta en Claude Code,
          describe lo que necesitas y él escribe el código.
        </p>
        <div class="flex gap-3 flex-wrap">
          <a
            href="#prompts"
            class="bg-primary text-on-primary text-[15px] font-semibold py-3 px-6 rounded-full no-underline transition-colors hover:bg-primary-hover"
            >Ver prompts de ejemplo</a
          >
          <a
            href="#stack"
            class="text-ink text-[15px] font-semibold py-3 px-6 rounded-full no-underline border border-line transition-colors hover:border-ink"
            >Qué incluye</a
          >
        </div>
      </section>

      <!-- Prompts de ejemplo -->
      <section id="prompts" class="border-t border-line scroll-mt-16">
        <div class="max-w-[960px] mx-auto px-8 py-16 max-sm:px-4 max-sm:py-12">
          <h2 class="font-serif text-[30px] font-semibold tracking-[-0.5px] leading-[1.15] mb-2">
            Prompts de ejemplo
          </h2>
          <p class="text-base text-muted leading-[1.6] mb-8 max-w-[560px]">
            Cópialos en Claude Code y ajústalos a lo que necesitas. Usa siempre datos ficticios o
            anonimizados.
          </p>
          <div class="grid grid-cols-2 gap-4 max-md:grid-cols-1">
            <div
              v-for="(prompt, i) in prompts"
              :key="prompt.titulo"
              class="flex flex-col gap-3 rounded-2xl border border-line bg-subtle p-5"
            >
              <div class="flex items-center justify-between gap-3">
                <h3 class="text-[15px] font-bold text-ink">{{ prompt.titulo }}</h3>
                <button
                  class="flex shrink-0 cursor-pointer items-center gap-1.5 rounded-full border border-line bg-bg px-3 py-1.5 text-[12px] font-semibold text-muted transition-colors hover:border-ink hover:text-ink"
                  :aria-label="`Copiar el prompt: ${prompt.titulo}`"
                  @click="copiar(prompt.texto, i)"
                >
                  <Check v-if="copiado === i" :size="13" aria-hidden="true" />
                  <Copy v-else :size="13" aria-hidden="true" />
                  {{ copiado === i ? "Copiado" : "Copiar" }}
                </button>
              </div>
              <p
                class="font-mono text-[13px] leading-[1.65] text-body bg-bg border border-line rounded-xl p-4"
              >
                {{ prompt.texto }}
              </p>
            </div>
          </div>
        </div>
      </section>

      <!-- Stack -->
      <section id="stack" class="border-t border-line scroll-mt-16">
        <div class="max-w-[960px] mx-auto px-8 py-16 max-sm:px-4 max-sm:py-12">
          <h2 class="font-serif text-[30px] font-semibold tracking-[-0.5px] leading-[1.15] mb-2">
            Para qué sirve cada cosa
          </h2>
          <p class="text-base text-muted leading-[1.6] mb-8 max-w-[560px]">
            No necesitas dominar estas piezas. Conocerlas te ayuda a explicarle a Claude qué quieres
            cambiar.
          </p>
          <div class="grid grid-cols-3 gap-4 max-md:grid-cols-2 max-sm:grid-cols-1">
            <div
              v-for="item in stack"
              :key="item.nombre"
              class="rounded-2xl border border-line p-5"
            >
              <component
                :is="item.icono"
                class="mb-4 text-ink"
                :size="20"
                :stroke-width="1.8"
                aria-hidden="true"
              />
              <h3 class="text-[15px] font-bold text-ink mb-1.5">{{ item.nombre }}</h3>
              <p class="text-sm text-muted leading-[1.6]">{{ item.texto }}</p>
            </div>
          </div>
        </div>
      </section>
    </main>

    <!-- Pie de página -->
    <footer class="border-t border-line py-7 px-6 text-center text-[13px] text-muted">
      © {{ year }} Claude Code para Todos
    </footer>
  </div>
</template>
