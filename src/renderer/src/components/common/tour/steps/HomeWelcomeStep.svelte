<script lang="ts">
  import icon from '@resources/img/icon.png'

  import { _ } from 'svelte-i18n'

  // Une teinte de la palette produit par fonctionnalité : la même grammaire
  // que les tuiles de création de l'accueil. Les clés restent littérales
  // pour l'extraction i18n.
  const features = [
    {
      icon: 'ph-stack',
      tint: 'var(--brand-tint-document)',
      tile: 'var(--brand-tile-document)',
      title: 'Unified Creative Workspace',
      text: 'Write documents, build spreadsheets, create presentations, design visuals, and structure data — all inside the same living file.'
    },
    {
      icon: 'ph-shield-checkered',
      tint: 'var(--brand-tint-form)',
      tile: 'var(--brand-tile-form)',
      title: 'Privacy & Ownership First',
      text: 'Your files remain under your control. Dynadoc is designed around data privacy, local-first workflows, encryption, and sovereign ownership.'
    },
    {
      icon: 'ph-database',
      tint: 'var(--brand-tint-spreadsheet)',
      tile: 'var(--brand-tile-spreadsheet)',
      title: 'Structured & Connected Data',
      text: 'Variables, references, tables, layouts, and components stay synchronized across your entire workspace in real time.'
    },
    {
      icon: 'ph-cloud-arrow-down',
      tint: 'var(--brand-tint-presentation)',
      tile: 'var(--brand-tile-presentation)',
      title: 'Sovereign Infrastructure',
      text: 'Choose where your data is stored — cloud, private infrastructure, regional servers, or on-premise deployment.'
    },
    {
      icon: 'ph-sparkle',
      tint: 'var(--brand-tint-design)',
      tile: 'var(--brand-tile-design)',
      title: 'AI Integrated Natively',
      text: 'Automate repetitive work, generate content, analyze data, and accelerate workflows with integrated AI assistance.'
    }
  ]
</script>

<div class="flex flex-col gap-5">
  <!-- Hero : le logo, le nom et la promesse, sur un fond teinté accent. -->
  <div
    class="relative overflow-hidden flex items-center gap-4 p-5 rd-2xl border border-[--theme-border-subtle] bg-[--accent-soft]"
  >
    <img src={icon} alt="" class="w-12 h-12 shrink-0 relative z-10" />
    <div class="relative z-10 min-w-0">
      <h3 class="text-xl font-bold tracking-tight text-base-content">
        {$_('Welcome to dynadoc')}
      </h3>
      <p class="mt-1 text-sm font-semibold text-[--accent-text]">
        {$_('Text. Spreadsheets. Slides. Design. In one document.')}
      </p>
      <p class="mt-2 text-xs leading-relaxed text-base-content/70 italic max-w-lg">
        "{$_(
          'A new productivity workspace built for structured knowledge, collaborative creation, and sovereign data ownership.'
        )}"
      </p>
    </div>

    <!-- Background "Blueprint" Decor -->
    <i class="ph ph-cpu absolute -right-4 -bottom-4 text-7xl opacity-5 rotate-12"></i>
  </div>

  <!-- Fonctionnalités : une grille régulière, la dernière sur toute la
       largeur. Titres en casse normale et textes lisibles (12 px, 70 %). -->
  <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
    {#each features as feature, i}
      <div
        class="welcome-feature flex items-start gap-3 p-3 rd-xl border border-[--theme-border-subtle] {i ===
        features.length - 1
          ? 'sm:col-span-2'
          : ''}"
        style="--tint:{feature.tint}; --tile:{feature.tile}"
      >
        <span
          class="welcome-feature__icon shrink-0 flex items-center justify-center w-9 h-9 rd-[11px]"
          aria-hidden="true"
        >
          <i class="ph-bold {feature.icon} text-[17px]"></i>
        </span>
        <div class="min-w-0">
          <div class="text-[13px] font-semibold text-base-content">{$_(feature.title)}</div>
          <div class="mt-0.5 text-xs leading-relaxed text-base-content/70">
            {$_(feature.text)}
          </div>
        </div>
      </div>
    {/each}
  </div>

  <!-- Dynamic Tip/Action -->
  <!-- <div
    class="mt-2 flex items-center justify-between p-2 rd-md border border-dashed border-base-content/20 bg-base-200/50"
  >
    <div class="flex items-center gap-2 text-[9px] font-mono text-base-content/40">
      <i class="ph ph-command"></i>
      <span
        >{$_('Press')} <kbd class="kbd kbd-xs">Ctrl</kbd> + <kbd class="kbd kbd-xs">K</kbd>
        {$_('to search, navigate and access commands')}</span
      >
    </div>
    <div class="badge badge-outline badge-xs opacity-50 font-mono">v1.2.4-stable</div>
  </div> -->
</div>

<style>
  .welcome-feature {
    transition:
      background-color 180ms ease,
      border-color 180ms ease;
  }
  .welcome-feature:hover {
    background: var(--tint);
    border-color: color-mix(in srgb, var(--tile) 30%, transparent);
  }
  .welcome-feature__icon {
    background: var(--tint);
    color: var(--tile);
  }
  .welcome-feature:hover .welcome-feature__icon {
    background: var(--color-base-100);
  }
</style>
